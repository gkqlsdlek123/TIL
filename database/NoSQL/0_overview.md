# 📚 NoSQL

※ 아래의 포스팅은 ACADEMIND의 SQL vs NoSQL or MySQL vs MongoDB 내용을 발췌했다.

👉🏻 https://academind.com/learn/web-dev/sql-vs-nosql/



## NoSQL ?

NoSQL이 무엇의 약자인지는 사람에 따라 NoSQL, Not Only SQL, Non-Relational Operational Database SQL로 엇갈리는 의견들이 있지만, 현재 **Not Only SQL**로 풀어 설명하는 것이 다수를 차지하고 있다.

이 말의 의미를 풀어보면, 기존의 **관계형 DBMS가 갖고있는 특성 뿐만 아니라 다른 특성들을 부가적으로 지원한다는 것**을 의미한다.

> 관계형 모델을 사용하지 않으며 테이블 간 연결해서 조회할 수 있는 조인 기능이 없음

> 데이터 조회를 위해 직접 프로그래밍하는 등의 비 SQL 인터페이스를 통한 데이터 접근

> 대부분 여러 데이터베이스 서버를 묶어서(클러스터링) 하나의 데이터베이스를 구성

> 관계형 데이터베이스에서는 지원하는 데이터 처리 완결성(Transaction, ACID 지원)이 보장되지 않음

> 데이터의 스키마와 속성들을 다양하게 수용하고 동적으로 정의(Schemaless)

> 데이터베이스의 중단없는 서비스와 자동 복구 기능 지원

> 대다수의 제품이 Open Source로 제공

> 대다수의 제품이 고 확장성, 고 가용성, 고 성능 특징을 가짐



## 특징
NoSQL 데이터베이스는 기존의 관계형 데이터베이스보다 더 융통성있는 데이터 모델을 사용하고 데이터의 저장 및 검색을 위한 특화된 메커니즘을 제공한다.

이를 통해 NoSQL 데이터베이스는 단순 검색 및 추가작업에 있어서 매우 최적화된 키 값 저장 기법을 사용하여 응답속도나 처리효율 등에 있어서 매우 뛰어난 성능을 나타낸다.



## 종류
저장되는 데이터의 구조(Data Model)에 따라 아래와 같이 나누어 볼 수 있다.

#### 1. Key-Value DB(널리 쓰임)
Key와 Value의 쌍으로 데이터가 저장되는 유형으로써 Amazon의 Dynamo Paper에서 유래되었다.
KEY값과 VALUE값으로 구분된 하나의 묶음으로 저장되는, 아주 익숙한(JSON) 구조의 데이터.
단순한 구조이기에 속도가 빠르고, 분산 저장이 용이하다는 장점이 있다.
단순한 객체에서 복잡한 집합체까지 무엇이든 **KEY-VALUE**값이 될 수 있고 다른 유형의 데이터베이스로는 불가능한 범위까지 수평 확장을 할 수 있게 해 준다. 
상당한 유연성을 제공하며 동일 데이터에서 메모리를 훨씬 덜 사용하므로 부하관리에 큰 이점이 있을 수 있다.

> Redis, Oracle NoSQL Database, Riak, Vodemort, Tokyo등의 제품이 알려져 있다.

![image-20201208070321974](/Users/habin_kim/Development/TIL/image-20201208070321974.png)



#### 2. Wide Columnar DB
Big Table DB라고도 하며, Google의 BigTable Paper에서 유래되었습니다. Column Family 데이터 모델을 사용하고 있다.

> HBase, Cassandra, Hypertable이 이에 해당된다.

![image-20201208065520259](/Users/habin_kim/Development/TIL/image-20201208065520259.png)



#### 3. Document DB
Lotus Notes에서 유래되었으며, JSON, XML과 같은 Collection 데이터 모델 구조를 채택하고 있다. 
테이블의 스키마가 상당히 유동적으로 이루어질 수 있어서 레코드마다 각각 다른 스키마를 가질 수 있다.
XML이나 JSON같은 도큐먼트를 이용해서 레코드를 저장한다 하여 DOCUMENT DATABASE라고 불린다.
트리형 구조로 데이터를 만들고 조회하기 딱 좋은 데이터베이스다.

> Mongo DB, Cough DB가 이 종류에 해당된다.

![image-20201208065546890](/Users/habin_kim/Development/TIL/image-20201208065546890.png)



#### 4. Graph DB
Euler & Graph Theory에서 유래한 DB다. Nodes, Relationship, Key-Value 데이터 모델을 채용하고 있다. 
데이터를 노드로 표현하고, 노드 사이의 관계를 화살표로 표현하는 데이터베이스.
일반적으로 관계형 데이터베이스보다 퍼포먼스가 뛰어나며 유연한 데이터 처리와 유지보수가 용이한 것이 장점이다. SNS 구축에 사용하기에 용이하다.

> Neo4J, BlazeGraph, OrientDB 등의 제품이 있다.

![image-20201208065609964](/Users/habin_kim/Development/TIL/image-20201208065609964.png)



## SQL vs NoSQL

![SQL? NoSQL?](/Users/habin_kim/Development/TIL/SjzxDRfvpW_MGArz60_rYsHRrrg.png)

