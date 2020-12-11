# AOP : 에스팩트 지향 프로그래밍

전통 적인 객체지향 기술의 설계 방법으로는 독립적인 모듈화가 불가능한 트랜잭션 경계설정과 같은 부가 기능을 어떻게 모듈화 할것인 가를 연구해온 사람들은 이 부가 기능을 모듈화 작업은 기존 객체지향 설계 패러다임과 구분되는 새로운 특성이 있다고 생각 했다. 그래서 이런 부가 기능을 객체지향 기술에서 주로 사용하는 오브젝트와는 다르게 틀별할 이름을 부리기 시작했다 그것이 에스펙트다. 

**에스팩트란 그 자체로 애플리케이션의 핵심기능을 담고 있지는 않지만, 애플리케이션을 구성하는 중요한 한 가지 요소이고, 핵심 기능에 부가되어 의미를 갖는 특별한 모듈을 가르킨다.**

애플리케이션의 핵심 적인 기능에서 부가적인 기능을 분리해서 에스팩트라는 독특한 모듈로 만들어서 설계하고 개발하는 방법을 애스팩트 지향 프로그래밍 AOP라고 부른다. **AOP는 OOP를 돕는 보조적인 기술이지 OOP를 대체하는 새로운 개념은아니다.**


**AOP는 결국 애플리케이션을 다양한 측면에서 독립적으로 모델링하고, 설계하고 개발할 수 있도록 만들어주는 것이다.**

**OOP에서는 공통된 기능을 재사용하기 위한 방법으로 상속, 위임 같은 방법을 통해서 해결했지만 애플리케이션 전체에 부가 기능을 담당하는 로직들을 기존 OOP 방식으로는 해결하기 어렵다. 대표적으로 반복적인 캡슐화 처리가 있다. 이러한 문제를 AOP를 통해서 해결, 어플리케이션에 흩어져 있는 부가 기능 코드를 하나의 장소에서 관리하여 부가 기능이 핵심기능에 비 침투적으로 되어 서비스 모듈은 해당 기능에 충실하게 된다.**

## 프록시를 이용한 AOP

프록시로 만들어서 DI로 연결된 빈 사이에 적용해서 타깃 메서드 호출 과정에 참여해서 부가적인 기능을 제공해주도록 만들었다. 그래서 스프링 AOP는 자바 기본 JDK와 스프링 컨테이너 외에는 특별한 기술이나 환경을 요구하지 않는다. 

독립적으로 개발한 부가기능 모듈을 다야한 타깃 오브젝트의 메서드에 다이내믹하게 적용해주기 위해 가장 중요한 약할을 맡고 있는게 프록시다. 그래서 스프링 AOP는 프록시 방식의 AOP라고 할 수 있다.

## 바이트코드를 생성과 조작을 통한 AOP
가장 강력한 AOP 프레임워크로 꼽히는 AspjectJ는 프록시를 사용하지 않은 대표적인 AOP 기술이다. **AspjectJ는 프록시처럼 간접적인 방법이 아니라. 타깃 오브젝트를 뜯어 고쳐서 부가 기능을 집접 넣어주는 직접적인 방법을 사용한다.**

부가기능을 넣는다고 타깃 오브젝트의 소스코드를 수정할 수 없으니, **컴파일된 타깃의 클래스 파일 자체를 수정하거나 클래스가 JVM에 로딩되는 시점을 가로채 바이트코트를 조작하는 복잡한 방법을 사용한다.**

### 바이트 코트를 조작하는 이유

* 바이트코드를 조작해서 타깃 오브젝트를 직접 수정해버리면 스프링과 같은 DI 컨테이너의 도움을 받아서 자동 프록시 생성 방식을 사용하지 않아도 AOP를 적용 할 수 있다.
* 바이트 코드를 직접 조각해서 AOP를 적용하면 오브젝트의 생성, 필드 값의 조회와 조작, 스태틱 초기화 등 다양한 작업에 부가 기능을 부여해줄 수 있다. 이런 작업들은 프록시 방식에서는 불가능하다.
    * private 메서드 호출, 스태틱 메서드 호출 및 초기화 등등 적용이 가능하다


# AOP 용어 정리

## 타깃
타깃은 부가기능을 부여할 대상이다. 핵심 기능을 담은 클래스일 수도 있지만 경우에 따라서는 다른 부가기능을 제공하는 프록시 오브젝트일 수도 있다.

## 어드바이스
어드바이스는 타깃에게 제공할 부가기능을 담은 모듈이다. 어드바이스는 오브젝트로 정의하기도 하지만 메소드 레벨에서도 정의할 수 있다.

## 조인 포인트
조인 포인트는 어드바이스가 적용될 수 있는 위치를 말한다. **스프링의 AOP에서 조인 포인트는 메서드 실행 단계 뿐이다.**

## 포인트 컷
포인트컷이란 어드바이스를 적용할 조인포인트를 선별하는 작업 또는 그 기능을 정의한 모듈을 말한다.

## 프록시
프록시는 클라이언트와 타깃 사이에 투명하게 존재하면서 부가기능을 제공하는 오브젝트다. DI를 통해 타깃 대신 클라이언트에게 주입되며, 클라이언트의 메소드 호출을 대신 받아 타깃에 위임해준다. 이때 부가기능을 부여한다.

## 어드바이저
어드바이저는 포인트컷과 어드바이스를 하나씩 갖고 있는 오브젝트이다. 어드바이저는 어떤 부가기능(어드바이스)을 어디에(포인트컷) 전달할 것인가를 알고 있는 AOP의 가장 기본이되는 모듈이다.


# 참고
* [기억보단 기록을 AOP(3)](https://jojoldu.tistory.com/71)
* [토비의 스프링 VOL1](http://www.yes24.com/24/goods/7516721)