## Cassnadra 의 기본 특징 정리

NoSQL 데이터베이스의 한 종류인 Cassnadra 의 기본 특징에 대하여 알아봅시다.



### 데이터모델

- Key space
- Table
- Row
- column name : column value
  - **SET, LIST, MAP** 도 칼럼에 저장 가능

#### (참고)

- **Cassandra : Key-space > Table > Row > Column name : Column value**
- Mongodb : db > collection > document > key:value
- RDBMS : DB > Table > row > column
- elasticsearch : index > type> document > key : value



### Java 기반



### Column-family(Wide-Column) key-value store

- 하나의 row는 여러개의 column을 가질 수 있다.
- 각각의row가 같은 수의 column을 가질 필요는 없다. (Sparse Multidimensional Hash Table)
- 각 row는 unique key를 가진다. partitioning에 사용됨.
- No relational
- Map< RowKey, SortedMap<ColumnKey, ColumnValue> >와 같은 형태로 저장됨.



### Schemaless (Schema-free)

- 스키마란?
  - 데이터베이스를 구성하는 개체(Entity), 속성(Attribute), 관계(Relationship) 및 데이터 조작 시에 데이터 값들이 갖는 제약조건 등에 관해 전반적으로 정의하는 것이다.
  - Schema가 존재한다는 것은 그 구조가 미리 정의되어 있어야 한다는 의미. 이는 데이터의 급격한 변화에 대응하기 힘듬.
- Cassnadra 는 데이터 구조가 어떤 형태를 가질지, 어떤 fields를 가질지 미리 정의할 필요 없다.
- 데이터베이스에 저장된 Document는 각기 다른, 다양한 필드를 가질 수 있다.
- 각 필드는 서로 다른 데이터타입을 가질 수 있다.
- It actually mean dynamically typed schema.
  - 계속해서 필드의 추가 변경이 이루어지는 경우 유용함.
  - Agile development methodology 에 유용함
- Unstructured data 를 쉽게 저장할 수 있음
- 데이터 모델링을 한 다음 복잡한 join 문을 사용해서 쿼리하는 대신, 카산드라는 원하는 쿼리를 모델링한 다음 데이터를 제공하도록 함.



### CQL(Cassnadra Query Language) - SQL과 비슷한 쿼리 인터페이스

- CREATE TABLE , INSERT, SELECT 등 거의 유사함.



### CQL Key 용어정리

- ```
  partition key
  ```

  - partition key는 CQL 문법에서 Cassandra에 data를 **분산** 저장하기 위한 unique한 key
  - partition key는 특정 table을 구성할 때 반드시 1개 이상이 지정 되어야 하며, 여러 개 지정될 수도 있음
  - partition key가 단 1개일 경우
    - 해당 partition key로 지정된 CQL Column의 value가 실제 Cassandra Data Layer의 Row key로 저장됩니다.
  - partition key가 여러 개일 경우
    - 각 partition key로 지정된 CQL Column들의 value들을 “:”문자와 함께 조합한 값들이 실제 Cassandra Data Layer의 Row key로 저장됩니다.

- ```
  cluster key
  ```

  - Cassandra Data Layer에서 Row에 속한 모든 Column들은 항상 정렬(sor)된 상태로 저장
  - 따라서 cluster key는 이러한 **정렬에 대한 기준** 역할을 담당합니다.
  - cluster key로 지정된 CQL Column들의 value들은 나머지 Column들의 name 및 “:”문자와 함께 조합되어, 이 값이 실제 Cassandra Data Layer의 Column Name으로 저장됩니다.
  - 만약 cluster key가 전혀 없는 경우엔, CQL Column의 name이 그대로 Cassandra Data Layer의 Column Name이 됩니다.

- ```
  primary key
  ```

  - primary key는 CQL table에서의 각 row를 각자 unique 하게 결정해주는 기준 역할을 담당합니다.
  - `primary key`는 최소 1개 이상의 `partition key`와 0개 이상의 `cluster key`로 구성됩니다.

- ```
  composite key(=compound key)
  ```

  - 1개 이상의 CQL Column들로 이루어진 primary key를 composite key 혹은 compound key라고 부릅니다.

- ```
  composite partition key
  ```

  - composite partition key는 2개 이상의 다수의 CQL Column으로 이루어진 partition key를 의미합니다.

- Table은 최소 1개 이상의 Column을 `primary key`라는 것으로 지정해야 한다.

- Cassandra는 이렇게 `primary key`로 지정된 column들 중에서 `partition key`로 지정된 column의 value를 기준으로 **데이터를 분산**하게 됩니다.



### Cassandra Ring

- Cassandra는 `Gossip 프로토콜`을 통하여 **모든 노드가 동등**한 Ring 구조

- Cassandra는 기본적으로 Ring 구조를 띠고 있습니다.

- 그리고 Ring을 구성하는 각 노드에 Data를 분산하여 저장합니다

- `Partition Key`라고 불리는(실제 Cassandra Data Layer에서 Row Key라고 불리는) 데이터의 **hash값을 기준으로 Data를 분산**

- 처음 각 노드가 Ring에 참여하게 되면, Cassandra의 conf/cassandra.yaml에 정의된 각 설정을 통하여 각 노드마다 고유의 hash 값 범위를 부여 받음.

- 데이터의 `partition key(Row key)`의 hash 값을 계산하여 해당 데이터가 어느 노드에 저장되어 있는지알고 조회가능.

- Cassandra는 이렇게 계산된 `hash의 값`을 `token`이라고 부름

- 각각의 row 가 여러개의 column name : column value를 갖진다. 그 column 들 중에 `primary key`로 지정할 수 있음.

- `PRIMARY KEY (code, location)` <= 이렇게 하면 앞의 primary key가 `partition key` 가 되고, 뒤의 primary key가 `cluster key`가 된다.

- Cassandra Data Layer에서의

   

  ```
  Row key
  ```

   

  는 CQL partition key의 value이다

  - 복수의 partition key라면 해당 Column value들과 “:”문자의 조합이다.



#### `Gossip 프로토콜` 이란?

- 클러스터에 있는 노드 간에 정보를 공유 하는 프로토콜을 말한다.
- virus가 퍼지는 것 처럼 정보확산.
- lightweight in large groups
- spreads a multicast quickly
- highly fault-tolerant
- 



### 분산 (Shard)

- **카산드라는 마스터 없이 동작한다.**

- 즉, 데이터 분산이나 복구를 관장하는 별도의 서버가 없다.

- Row key를 token으로 변환해주는 모듈을 `Partitioner`라고 부릅니다

- RandomPartitioner, Murmur3Partitioner, ByteOrderedPartitioner

  라는 이름의 세 가지 Partitioner를 제공

  - **RandomPartitioner**는 Row key를 MD5로 hashing하여 token을 생성

  - **Murmur3Partitioner**는 Murmur5로 Hashing하여 token을 생성

  - BOP

    는 Row key를 16진수 형태로 변환하여 이 값을 token으로 사용,

    - BOP 변환된 token은 문자 순서로 정렬되어 각 노드에 분산
    - Row key들이 항상 문자 순서대로 정렬되어있으니 대용량 데이터를 특별한 가공 없이 그대로 range query를 할 수 있음
    - ByteOrderedPartitioner를 사용 시 Hotspot 발생의 예. A~G로 시작하는 Row key가 과도하게 많을 경우 노드 하나에 비대한 데이터가 쌓이게 된다

- Cassandra는 `Murmur3Partitioner`를 default Partitioner로 사용하고

- 모든 데이터를 비교적 균일하게 모든 노드에 분산



### Replication

- 처음 Keyspace 생성 할 때 Replication의 배치 전략과 그 전략에 맞는 Replication 복제 개수, 위치 위치를 결정 할 수 있음
- 이러한 기능을 지원하기 위해서 conf/cassandra.yaml의 endpoin_snitch 항목에 snitch의 종류를 세팅
- no single point of failure 을 위해서는 replication factor 는 3개가 되어야 함.
- 두 가지 `Replication Strategy`
  - SimpleStrategy
    - 하나의 Data Center를 이용할때. Ring 구조에서 시계방향으로 노드가 선택되어짐.
  - NetworkTopologyStrategy
    - 두 개 이상의 Data Center를 이용할 때.



#### topology 란

- 토폴로지의 일반적인 의미는 물리적인 배치의 형태로 이루어진 어떤 현장의 종류를 설명하는 것이다.
- 그러나 통신 네트웍을 논하는 맥락의 토폴로지란, 노드들과 이에 연결된 회선들을 포함한 네트웍의 배열이나 구성을 개념적인 그림으로 표현한 것이다.
- 대표적인 네트웍 토폴로지들을 예로 들면, 성형, 망형, 버스형, 환형, 나무형 등이 있다.

#### snitch란

- 데이터센터가 어떻게 구성되어있는지, 장비가 설치된 렉이 어떻게 나뉘어져 있는지에 대한 topology를 Cassandra에게 알려주기 위한 옵션



### INDEX (인덱스)

- Indexing 된 칼럼이 아니라면, 카산드라는 해당 칼럼을 가지고 filter 할 수 없다.
- (그게 primary key가 아니면 말이다.)
- `primary key`는 그 자체가 이미 indexed 되어있는 key 이기 때문에 index로 지정안해도 된다.
- collections 에서는 index를 제공해주지 않는다.
- 해당 칼럼이 인덱스로 지정되었다면, 새로운 데이터가 insert될때 indexed 된다.

#### index생성

- 아래와 같은 양식으로

  ```
  Create index IndexName on KeyspaceName.TableName(ColumnName);
  ```

- 예) university.student의 dept 칼럼을 index로 생성하겠습니다.

  ```
  create index DeptIndex on university.student(dept);
  ```

- 이제 해당 칼럼을 가지고 요리조리 조회를 할 수 있다!

#### index drop

- 아래와 같은 양식으로

  ```
   Drop index IF EXISTS KeyspaceName.IndexName
  ```





------

### Distributed and Decentralized

- **Distributed** Muliple machines 에서 작동 가능하도록 최적화 되어있다.
- **Decentralized** : 모든 노드가 동등하다.(Every node is identical)
- 노드 간에 Master-Slave 관계가 없다.
- 노드 상태 정보를 공유하기 위해 Gossip Protocol을 사용하는 Peer to Peer 아키텍처.
- 특정 노드가 특정한 역할을 하는 것이 아니므로 **No single point of failure**
- 이는, 따라서 카산드라가 **High Availability**를 가진다.
- 모든 노드가 같은 역할을 하므로 마스터 슬레이브 구조를 갖는 것보다 사용하기 간단하다.
- 그래서 확장하기가 쉽다. (50 개의 노드를 하는 것과 1개 노드를 하는 것이 크게 다르지 않다.)
- Any node(그 어떤 노드)에서나 Read / write를 수행가능.
- (참고)
  - MySQL, 몽고DB, 빅테이블과 같은 데이터저장소는 확장을 위해서 일부 노드를 슬레이브(Slave)로 두고 Centralize 해야함.
  - (Master-slave 형태로 Replication을 가지는 경우)
  - 이 경우 마스터 노드가 fail 할 경우 전체 데이터베이스가 위험해진다.
- 로드밸런싱
  - consistent hashing 방식에 로드밸런싱을 위한 알고리즘을 더하여 요청이 적은 서버를 요청이 많은 서버 구간으로 옮김.
  - 대량의 요청이 소수의 서버에 몰리는 것을 막아 장애 방지
  - 성능 향상
  - 신규로 서버가 추가되어도 로드밸런싱을 고려하여 요청이 많은 구간에 우선 추가



### Elastic Scalability (탄력적 확장성)

- Scalability

   

  (확장)

  - 성능저하가 거의 없이 시스템이 방대한 양의 request를 처리할 수 있도록 하는 구조적 기능.

  - `Vertical scaling` 은 단순히 기존의 머신에 메모리와 같은 하드웨어 용량을 추가 하는 것

  - ```
    Horizontal scaling
    ```

     

    은 Machines 를 추가해서 데이터를 분리해서 가지도록 하고, 하나의 머신이 모든 request에 대한 부담을 가지는 것을 피하는 방법.

    - (이 경우 다른 노드들과 데이터 sync를 유지하기 위한 머케니즘을 가져야함. )

- 매끄럽게 Horizontally scale up과 scale back down이 가능.

  - 즉, node 수를 추가하고 감소시키는 것이 쉽다. 매끄럽다.
  - 전체 클러스터를 재구성할 필요 없다.
  - 전체 데이터를 직접 rebalance 할 필요 없다.
  - 새로운 노드가 추가되면 클러스터의 일정 부분을 담당하게 되고
  - 노드에 장애가 발생하면 책임이 클러스터의 다른 노드에게 균등하게 분산된다.
  - 이는 `nodetool` 이라는 명령어로 쉽게 조정 가능하다.

- linear scalabliity

  - node를 추가하는 것은 Performance(짧은 시간동안의 처리량) 와 Throughput(장시간동안의 처리량)을 linearly(선형적으로) 향상시킨다.
  - (선형적 증가란? : 한 방향으로 직선적 진행, 원인이 많이 증가하면 결과도 많이 증가 )



### High Availability and Fault Tolerance

- **Availability(가용성) 란?** : 모든 DB 클라이언트는 항상 데이터 읽기와 쓰기가 가능해야한다.
- 컴퓨터는 하드웨어 컴포넌트의 결함 및 네트워크 중단 등 여러가지 오류가 발생가능
- Failed nodes 를 downtime(컴퓨터가 작동을 멈추는 시간) 없이 교체할 수 있다. 이는 모든 노드가 동등한 구조 덕분.
- Replication
  - Cassandra Ring 에서 서로 다른 머신의 노드에 Data를 복제한다.
  - Multiple Data Center 에 데이터 복제 가능
- 여러 데이터 센터를 구성 가능
  - local performance를 향상시킬 수 있고
  - 자연 재해에 대비 가능 (no downtime)
  - No single point of failure due to the p2p architecture



### High Performance

- Cassandra는 멀티 프로세서 / 멀티 코어 머신을 최대한 활용하고 여러 데이터 센터에 수용된 수십 개의 머신을 실행하도록 특별히 설계됨.
- 더 많은 서버를 추가 할때 성능을 저하시키지 않는다.
- **모든 disk write이 Sequential이다. (append only operation)**
- (참고) - [Cassandra 가 MySQL보다 빠른 이유 : lookup 하지 않고 그저 write 만 하기 때문](https://www.quora.com/Why-are-the-writes-of-Cassandra-executed-faster-than-MySQLs)
- no reading before writing



### Tunable Consistency

- Consistency(일관성)란?

   

  : 같은 시점의 requests 에 대해 동일한 결과(가장 최신 결과)를 리턴해야한다.

  - 모든 DB 클라이언트는 같은 쿼리에 대해 같은 값을 읽어야함.
  - 업데이트가 동시에 진행되는 상황에서도 만족해야함 (두명이 거의 동시에 하나 남은 상품을 구매할 때 약간이라도 늦게 클릭한 사람은 품절이라고 알려줄 수 있어야함 )
  - 항상 가장 최근에 기록된 값을 읽는다.
  - 같은 데이터를 가진 모든 노드에서 쓰기 상태가 일관될 때 보장됨.

- 하지만 Data store를 확장하는 것에는 Data **Consistency**, Node **Availability**, **Partition Tolerance** 사이에 trade-offs 를 동반한다.

- 카산드라는 전체 **Availability(가용성)**을 높이기 위해 **Consistency(일관성)**을 약간 희생한다.

- Consistency Level을 조정해서 Availability 와의 균형을 **조정할 수 있다.** *High Available 이냐 Strong Consistency냐

- Consistency level을 조절할 수 있다.

  - Strict Consistency
    - Sequential Consistency 라고도 한다.
    - 가장 엄격한 consistency 레벨
    - 어느 read 요청이나 가장 최신의 written value를 반환한다.
    - single processor machine에서는 쉽게 가능하지만 지역적으로 분산된 데이터센터에서 모든 operations 에 대해 global clock time stamp 가 필요하다.
    - 모든 업데이트는 동기적으로 수행되어야 한다
    - 업데이트가 완료될 때까지 blocking, locking 이 필요함
    - 클라이언트는 waiting time 이 발생한다.
    - failure 동안(다운됐을 때) 데이터가 완전히 unavailable 한 경우가 발생한다. 완벽하게 데이터가 consistent할 때까지 unavailable 하다.
  - Causal consistency
    - Weak (eventual) consistency
    - 모든 업데이트가 모든 분산 시스템에 적용되는데 약간의 시간이 필요함
    - 최종적으로는 모든 Replica 가 consistency를 가진다.

- Replication Factor & Consistency Level « 요거 다 시 조사 !

  - read 요청이 올 경우 해당 데이터를 가지고 있는 여러 Replication server에서 결과값을 비교해보고 time stamp가 최신인 것을 반환, 그리고 최신화.

  - Consistency Level 이

     

    ```
    All
    ```

     

    일 경우

    - 읽기 및 쓰기 요청이 발생할 경우 복제된 데이터를가진 모든 서버에서 응답을 보내와야 완료됩니다.
    - 이 경우 유연성 및 성능은 떨어지지만 데이터 신뢰도는 높아진다.

  - Consistency Level이

     

    ```
    One
    ```

    일경우

    - 읽기 및 쓰기 요청이 발생하였을 때 해당 데이터를 가지고 있는 하나의 서버 대상으로 해당 연산 처리
    - 데이터 신뢰성은 떨어지게되지만 분산에 대한 유연도와 응답 성능(속도)은 높아집니다.

  - Consistency Level이

     

    ```
    QUORUM
    ```

    일 경우

    - 읽기 및 쓰기 요청이 들어온 데이터를 복제하여 가지고 있는 일정 대술의 서버에서 응답을 보낼 경우 완료하게 됩니다.
    - Facebook에서 권장하는 Consistency Level은 One이나 replication factor(복제 서버수)를 3으로 둔 QUORUM

- (참고)- [Cassandra Replication 설정 (한글 블로그 정리)](https://teragoon.wordpress.com/2012/02/14/cassandra-replication-설정/)

- (참고) -[datastax configuring data consistency](http://docs.datastax.com/en/archived/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html)



### 누가 사용하는가

- Twitter : 분석 용도로 사용
- Mahalo : 실시간에 가까운 데이터 저장소로 사용
- Facebook : 받은 편지함 검색에 사용
- Digg : 거의 실시간에 가까운 데이터 저장소로 사용
- Rackspace : 클라우드 서비스, 모니터링, 로깅에 사용
- Reddit : 영속적인 캐시로 사용
- Cloudkick : 통계 및 분석 모니터링에 사용
- Ooyala : 거의 실시간에 가까운 비디오 분석 데이터 서비스와 저장에 사용
- SimpleGeo : 실시간 위치 인프라 스트럭처를 위한 주 데이터 저장소로 사용
- Onespot : 주 데이터 저장소의 서비스셋으로 사용
- 참고 - [More haste, less speed](http://colossus.tk/166)



### 복잡한 조건 검색이 불가능하다.



### 데이터 갱신 및 입력시 Atomic한 처리가 어렵다.





## 참고

- [Scalability, Performance, Availability and Maintainability of Cassandra](http://www.rapidvaluesolutions.com/tech_blog/cassandra-the-right-data-store-for-scalability-performance-availability-and-maintainability/)
- [Apache Cassandra NoSQL Performance Benchmarks(datastax)](https://academy.datastax.com/planet-cassandra/nosql-performance-benchmarks)
- [A Brief Introduction to Apache Cassandra(datastax)](https://academy.datastax.com/resources/brief-introduction-apache-cassandra)
- [Apache Cassandra 톺아보기 - 1편](http://meetup.toast.com/posts/58)
- [MongoDB vs Cassandra Performance(조대협의 블로그)](http://bcho.tistory.com/624)
- [Cassandra 특징 간단 정리](http://colossus.tk/165)
- [NoSQL 가용성과 운영 안정성(네이버D2)](http://d2.naver.com/helloworld/1039)
- [Cassandra Introduction & Features(slide-share)](https://www.slideshare.net/planetcassandra/cassandra-introduction-features-30103666)
- [간단하게 mongo와 cassandra , hbase잘 정디된 블로그](http://blog.naver.com/PostView.nhn?blogId=samuelc&logNo=20186928327)
- [Cassandra Introduction& Feature](https://www.slideshare.net/planetcassandra/cassandra-introduction-features-30103666)
- [Cassandra.pdf](https://cs.uwaterloo.ca/~tozsu/courses/CS848/W15/presentations/Cassandra.pdf)
- [학술자료(영문)](http://salsahpc.indiana.edu/b534projects/sites/default/files/public/1_Cassandra_Malae%2C Naga Rekha.pdf)
- [영문 자료 상세함.](https://www.safaribooksonline.com/library/view/cassandra-the-definitive/9781449399764/ch01.html)
- [카산드라 특징 정리 블로그포스팅](http://clearpal7.blogspot.kr/2016/06/cassandra_15.html)

------

(다시 더 읽어볼 것 )

- [Cassandra: Daughter of Dynamo and BigTable](https://blog.insightdatascience.com/cassandra-daughter-of-dynamo-and-bigtable-1b57b16229b9)