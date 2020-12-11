# Strategy-pattern

## 인터페이스
* 기능에 대한 **선언과 구현 분리**
* 기능을 사용 **통로**

## 스트레지 패턴
* 여러 알고리즘을 하나의 **추상적인** 접근점을 만들어 접근 점에서 서로 **교환 가능** 하도록 하는 패턴
* 스트래지 패턴은 전략을 숩게 바꿀 수 있도록 해주는 디자인 패턴이다. 여기서 전략이란 어떤 목적을 당성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘 등으로 이해할 수 있다.

## 스트래지 패턴 역할
* Strategy : 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시한다.
* ConcreteStrategy : 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스다.
* Context: 스트래지 패턴을 이용하는 역할을 수행한다. 필요에 따라 동적으로 구체적인 전략을 바꿀수 있도록 setter를 제공한다.(클라이언트를 말하는 거같다 뇌피셜90%)

## 문제점

### 중복 코드 발생
* 동일한 로직에 대해서 하위 레벨에서 정의 하여 중복 코드 발생
	- 미사일 공격 하는 기능이 있을시 마진가, 태권 브이 로봇이 동일하게 미사일 공격 기능이 있을시 각자 동일 한 코드를 정의
	- 상위 클래스에서 공통 로직 구현 하지니 상위 레벨은 하위 레벨의 상세합을 알게되니 DIP 위반 하게됨

### 새로운 코드 작성 및 기존 코드 변경 어려움
* 모든걸 하위 클래스에서 정의 하다보니 미사일 공격 코드가 변경이 발생하면 마진가, 태권 브이에서 모두 변경 해야함 OCP / LSP 위반
* 중복 코드 발생과 비슷한 이슈로 검 공격이 추가 되면 태권 브이, 마진가에서 모두 구현 해줘야함


## 해결
* 문제를 발생시키는 용인은 로봇의 이동 방식과 공격 방식의 변화다. 즉 새로운 방식 이동 및 공격 기능이 계속 추가될 수 있도록 기존의 로봇이나 새로운 로봇이 이러한 기능을 별다른 코드 변경 없이 제공받거나 기존의 공격이나 이동방식을 다른 공격이나 이동 방식으로 쉽게 변경 할수 있어야 한다.
* 즉 MovingStrategy, AttackStrategy 인터페이스를 만들고 상세 합의 미사일 클래스, 펀치 클래스의 하위 클래스를 만들어야함

* **이런 설계는 한번에 나오는 것이 아니라 변화된 것을 찾은 후에 이를 클래스로 캡슐화 해야함 즉 점진적으로 개선하는 방법임, 그래서 TDD가 중한거 같음 구조 뜯어 고칠때 TDD 코드가 있으면 진짜 좋음**