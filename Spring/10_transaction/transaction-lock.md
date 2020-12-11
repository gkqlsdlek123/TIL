> 해당 자료는 [DDD Start](http://www.yes24.com/Product/Goods/27750871?scode=029)를 보고 정리한 자료입니다. 정말 추천드리는 책입니다. 

# 트랜잭션 잠금
한 주문 애그리거트에 대해 운영자는 배송 준비 상태로 변경할 때 사용자는 배송지 주소를 변경하면 어떻게 될까? 아래 그림은 운영자와 고객이 동시에 한 주문 애그리거트를 수정하는 과정을 보여준다. (배송 상태로 변경되면 더 이상 배송지 변경은 할 수 없다.)

![](../../assets/transaction-thread.png)

운영자 스레드와 고객 스레드는 같은 주문 애그리거트를 나타내는 다른 객체를 구하게 된다 (트랜잭션 마다 리포지토리라는 새로운 애그리거트를 생성한다.) 운영자 스레드와 고객 스레드는 개념적으로 동일한 애그리거트이지만 물리적으로 서로 다른 애그리거트 객체를 사용한다.

때문에 운영자 스레드가 주문 애그리거트 객체 배송 상태로 변경하더라도 고객 스레드가 사용하는 주문 애그리거트 객체에는 영향을 주지 않는다. 고객 스레드 입장에서 주문 애그리거트 객체는 아직 배송 상태 전이므로 배송지 정보를 변경할 수 있다.

이 상황에서 두 스레드는 각각 트랜잭션을 커밋할 때 수정한 내용을 DBMS에 반영한다. 즉 배송 상태로 바뀌고 배송지 정보로 바뀌게 된다. 이 순서의 문제점은 운영자는 기존 배송지 정보를 이용해서 배송 상태로 변경했는데 그 사이 고객은 배송지 정보를 변경했다는 점이다. 즉 애그리거트의 일관성이 깨지는 것이다. 이런 문제가 발생하지 않도록 하려면 다음 두 가지 중 하나를 해야한다.

* 운영자 배송지 정보를 조회하고 상태를 변경하는 동안 고객이 애그리거트를 수정하지 못하게 막는다. (수정하지 못하는 것보다 조회까지 막아야 된다고 생각함)
* 운영자가 배송지 정보를 조회한 이후 고객이 배송지 정보를 변경하면 운영자가 애그리거트를 다시 조회한 뒤 수정하도록 한다.

이 두 가지는 애그리거트 자체의 트랜잭션과 관련이 있다. DBMS가 지원하는 트랜잭션과 함께 애그리거트를 위한 추가적인 트랜잭션 처리 기법이 필요하다. **애그리거트에 대해 사용할 수 있는 대표적인 트랜잭션 처리 방식에는 선점 잠금과 비선점 잠금의 두 가지 방식이 있다.**

## 선점 잠금
선점 잠금은 먼저 애그리거트를 구한 스레드가 애그리거트 사용이 끝날 때까지 다른 스레드가 해당 애그리거트를 수정하는 것을 막는 방식이다.

![](../../assets/transaction-thread2.png)

스레드1 선점 잠금방식으로 애그리거트를 구한 뒤 이에서 스레드2가 같은 애그리거트를 구하고 있는데, 이 경우 스레드2는 스레드1이 애그리거트에대한 잠금을 해제할 때 까지 블로킹된다.

스레드1이 애그리거트를 수정하고 트랜잭션을 커밋하면 잠금을 해제한다. 이 순간 대기하고 있던 스레드2가 애그리거트에 접근하게 된다. 스레드1이 트랜잭션을 커밋 뒤에 스레드2가 애그리거트를 구하게 되므로 스레드2는 스레드1이 수정한 애그리거트의 내용을 보게된다.


한 스레드가 애그리거트를 구하고 수정하는 동안 다른 스레드가 수정할 수 없음므로 동시에 애그리거트를 수정할 때 발생하는 데이터 충돌 문제를 해소할 수 있다.

### 선점 잠금 적용

![](../../assets/transaction-thread3.png)

1. 운영자 스레드가 먼저 선점 잠금 방식으로 주문 애그리거트를 구함
2. 고객 스레드는 운영자 스레드가 잠금을 해제할 때까지 고객 스레드는 대기 상태
3. 운영자 스레드가 배송 상태로 변경한 뒤에 트랜잭션을 커밋하면 잠금을 해제한다.
4. 잠금이 해제된 시점에 고객 스레드가 구하는 주문 애그리거트는 운영자 스레드가 수정한 배송 상태의 주문 애그리거트이다. 배송 상태이므로 주문 애그리거트는 배송지 변경 시 에러를 발생하거 트랜잭션이 실패한다.
5. 고객은 `이미 배송이 시작되어 배송지를 변경할 수 없습니다.`와 같은 안내 문구를 받게 됨


선점 잠금은 보통 DBMS가 제공하는 행 단위 잠금을 사용해서 구현한다. 오라클을 비롯한 다수 DBMS가 for update와 같은 쿼리를 사용해서 특정 레코드에 한 사용자만 접근할 수 있는 잠금 장치를 제공한다.

JPA의 EntityManager는 LockModeType을 인자로 받는 `find()` 메서드를 제공하는데, `LockModeType.PESSIMISTIC_WRITE`를 값으로 전달하면 해당 엔티티와 매핑된 테이블을 이용해서 선점 잠금 방식을 적용할 수 있다.

```java
Order order = entityManager.find(Order.class, orderNo, LockModeType.PESSIMISTIC_WRITE)
```

JPA 프로바이더와 DBMS에 따라 잠금 모드의 구현이 다른데, 하이버네티으의 경우 `PESSIMISTIC_WRITE`를 잠금 모드로 사용하면 `for update` 쿼리를 사용해서 선점 잠금을 구현한다.

### 선점 잠금과 교착상태
선점 잠금 기능을 사용할 때는 잠금 순서에 따른 교착 상태가 발생하지 않도록 주의해야 한다. 예를 들어, 다음과 같은 순서로 두 스레드가 선점 잠금을 시도를 한다고 해보자

1. 스레드 1: A 애그리거트에 대한 선점 잠금 구함
2. 스레드 2: B 애그리거트에 대한 선점 잠금 구함
3. 스레드 1: B 애그리거트에 대한 선점 잠금 시도
4. 스레드 2 : A 애그리거트에 대한 선점 잠금 시도

이 두 스레드는 상대방 스레드가 먼저 선점한 잠금을 구할수 없어 더 이상 다음 단계를 진행하지 못하게 된다. **즉 스레드 1과 스레드 2는 교착상태에 빠지게 된다.**

선점 잠금에 따른 교착 상태는 상대적으로 사용자 수가 많을 때 발생할 가능성이 높고, 사용자 수가 많아지면 교착 상태에 빠지는 스레드가 더 빠르게 증가하게 된다. 더 많은 스레드가 교착 상태에 빠질수록 시스템은 점점 아무것도 할 수 없는 상황에 이르게 된다.

이런 문제가 발생하지 않도록 하려면 잠금을 구할 때 최대 대기 시간을 지정해야한다. JPA에서 선점 잠금을 시도할 때 최대 대기 시간을 지정하려면 다음과 같이 힌트를 사용하면 된다.

```java
Map<String, Object> hints = new HashMap<>();
hints.put("javax.persistence.lock.timeout", 2000);
Order order = entityManager.find(Order.class, orderNo, LockModeType.PESSIMISTIC_WRITE, hints);
```

JPA의 `javax.persistence.lock.timeout` 힌트는 잠금을 구하는 대기 시간을 밀리초 단위로 지정한다. 지정한 시간이내에 잠금을 구하지 못하면 익셉션을 발생 시킨다. 이 힌트를 사용할 때 주의할 점은 DBMS에 따라 힌트가 적용되지 않는 다는 점이다. 이 힌트를 이용할 때에는 사용중인 DBMS가 관련 기능을 지원하는지 확인해야 한다.

### 선점 잠금으로 해결 못하는 문제
REPEATABLE_READ은 트랜잭션이 지속되는 동안 다른 트랜잭션이 해당 필드를 변경할 수 없는 격리 레벨이다. **이것으로 위의 문제를 해결할 수 있지는 않다.**

![](../../assets/transaction-thread2.png)

위 그림은 Isolcation Level이 `REPEATABLE_READ`의 경우 **DB는 조회는 되었지만 아직 커밋하지 않은 로우에 읽기 잠금을 걸어둔 상태이다.** 즉 스레드 1이  애그리거트를 구하는 동안 접근 잠금을 걸고 애그리거트를 수정하는 동안 스레드2에서는 잠금 대기 상태이기 때문에 잠금된 동안은 블록킹이 된다.

**하지만 JPA 환경에서는 `REPEATABLE_READ`의로 해당 문제는 해결할 수 없다.**

JPA여서는 트랜잭션 마다 영속성 컨텍스트를 갖게 됩니다. 위 그림처럼 각각의 영속성 컨텍스트 1차 캐시에 저장된 주문 객체를 영속성 컨텍스트A는 배송 상태를 변경, 영속성 컨텍스트B는 주소를 변경하려고 한다. 결과적으로 모두 Commit이 되어 데이터베이스에 반영되게 된다.

**`REPEATABLE_READ`의 경우 아직 커밋하지 않은 로우에 읽기 잠금을 걸어둔 상태이지만 동일한 시점(거의 동일한) 조회한 데이터는 1차 캐시에 저장되기 때문에 `REPEATABLE_READ`의로 해결할 수가 없다.** 

그리고 선점 잠금으로 해결할 수 없는 것이 있다. 예를들어 A와 B가 동시에 제목이 같은 공지사항을 수정한다고 가정하자 **결과적으로 먼저 완료한 사용자의 수정사항은 사라지고 나중에 완료한 수정사항만 남게된다. 이것을 두 번의 갱신 분실 문제 second lost updates problem라 한다. 두 번의 갱신 분실 문제는 데이터베이스 트랜잭션의 범위를 넘어선다. 따라서 트랜잭션으로만 문제를 해결 할 수 없다.**

이 문제를 해결하기 위해서는 크게 3가지 방법이 있다.

1. 마지막 커밋만 인정하기: 먼드들어온 A의 내용을 무시하고 마지막에 커밋한 B의 내용만 인정한다.
2. **최초 커밋만 인정하기: 사용자 A가 이미 수정을 완료했으므로 사용자 B가 수정을 완료할 때 오류가 발생한다.**
3. 총돌하는 갱신 내용 병합하기: 사용자 A와 사용자 B의 수정사항을 병합한다.

최초 커밋만 인정하는 비선점 잠금에 대해서 알아보자

## 비선점 잠금
선점 잠금이 강력해 보이긴 하지만 선점 잠금으로 모든 트랜잭션 충돌 문제가 해결되는 것은 아니다. (위에서 설명했지만 전체 Flow 다시 정리)

![](../../assets/transacion-thread-desc.png)

1. 운영자는 배송을 위해 주문 정보를 조회한다.
2. 고객이 배송지 변경을 위해 변경 폼을 요청한다. 시스템은 변경 폼을 제공한다.
3. 고객이 새로운 배송지를 입력하고 폼을 전송해서 배송지를 변경한다.
4. 운영자가 1번에서 조회한 주문 정보를 기준으로 배송지를 정하고 배송 상태 변경을 요청한다.

여기서 문제는 운영자가 배송지 정보를 조회하고 배송 상태로 변경하는 사이에 고객이 배송지를 변경한다는 것이다. 운영자는 고객이 변경하기 전에 배송지 정보를 이용해서 배송 준비를 한 뒤에 배송 상태로 변경하게 된다.

**즉 배송 상태 변경 전에 배송지를 한 번 더 확인 하지 않으면 운영자는 다른 배송지로 물건을 발송하게 되고, 고객은 배송지를 변경했음에도 불구하고 엉뚱한 곳으로 주문한 물건을 받는 상황이 발생한다.**

이 문제는 선점 잠금 방식으로 해결 할 수 없는데, 이 때 필요한 것이 비선점 잠금이다. 비선점 잠금 방식은 잠금 을 해서 동시에 접근하는 것을 막는 대신 **변경한 데이터를 실제 DBMS에서 반영하는 시점에 변경 가능 여부를 확인하는 방식이다.**

비선점 잠금을 구현하려면 애그리거트에 버전으로 사용할 숫자 타입의 프로퍼티를 추가해야한다. 애그리거트를 수정할 때마다 버전으로 사용할 프로피터 값이 1씩 증가하는데, 이때 다음과 같은 쿼리를 사용한다.

```sql
UPDATE aggrable SET version = version +1, colx = ?, coly =?
WHERE aggid =? and version = 현재 버전
```
이 쿼리는 수정할 애그리거트와 매핑되는 테이블의 버전 값이 현재 애그리거트의 버전과 동일한 경우에만 데이터를 수정한다. 그리고 수정에 성공하면 버전 값을 1증가 시킨다. 따라서, **다른 트랜잭션이 먼저 데이터를 수정해서 버전 값이 바뀌면 데이터 수정에 실패하게 된다.**

### 비선점 잠금을 이용한 트랜잭션 충돌 방지

![](../../assets/transacion-thread4.png)

1. 스레드 1 애그리거트 조회
2. 스레드 2 애그리거트 조회
3. 스레드 1 애그리거트 수정 시도 (스레드 2 보다 먼저 시도한다), 수정에 성공하고 버전은 6이 된다.
4. 스레드 2 애그리거트 수정 시도, 이미 애그리거트 버전이 6이므로 스레드2는 데이터 수정에 실패하게 된다.

JPA는 버전을 이용한 비선점 잠금을 기능을 지원한다. 다음과 같이 버전으로 사용할 필드에 `@Version` 애노테이션을 붙이거 매핑되는 테이블 버전을 지정한 칼럼을 추가하기만 하면된다.

```java
@Entity
@Table(name = "purchase_order")
public class Order {
    ...
    @Version
    private long version;
}
```

JPA는 엔티티가 변경되어 UPDATE 쿼리를 실행할 때 `@Version`에 명시한 필드를 이용해서 비선점 잠금 쿼리를 실행한다. 즉 애그리거트 객체의 버전 10이면 UPDATE 쿼리를 실행할 때 다음과 같은 쿼리를 사용해서 버전 일치하는 경우에만 데이터를 수정한다.

```sql
update purchase_order SET ..., version = version + 1
where number ? and version = 0;
```

응용 서비스 버전에 대해 알 필요가 없다. 리포티터리에 필요한 애그리거트를 구현하고 알맞은 기능을 실행하면 된다. 기능을 실행하는 과정에서 애그리거트의 데이터가 변경되면 JPA트랜잭션 종료 시점에 비선점 잠금을 위한 쿼리를 실행 한다.

```java
@Controller
public class OrderController {
    private ChangeShppingService changeShippingService;

    @PutMapping("/shpping")
    public String changeShipping(ChangeShippingsRequest changeReq){
        try {
            changeShppingService.changeShpping(changeReq);
            return "changeShppingSuccess";
        } catch (OptimistickLockingFailureException ex){
            // 누군가 먼저 같은 주문 애그리거트를 수정 했음으로
            // 트랜잭션 충돌이 일어났다는 메시지를 보여준다.
            return "changeShppingTxConflict";

        }
    }
}
```

### 비선점 트랜잭션 충돌 문제 해결 Flow
![](../../assets/transacion-thread-desc2.png)

## 참고
* [DDD Start](http://www.yes24.com/Product/Goods/27750871?scode=029)
* [자바 ORM 표준 JPA 프로그래밍](http://www.acornpub.co.kr/book/jpa-programmig)