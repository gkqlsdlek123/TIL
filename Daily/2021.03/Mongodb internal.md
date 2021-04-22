# 몽고디비의 내부 구조 (Mongodb internal)



## Storage Engine 의 종류

- WierdTiger (3.2 default)
  - Many CPU, Lots of RAM 에 효과적
- MMAPv1 (3.0 default)

## (1) MMAPv1 스토리지엔진의 구조



### DB internal structure

![mongodb_internal_structure](http://nicewoong.github.io/assets/mongodb_internal_structure.png)

- MongoDB의 데이터 저장소는 메모리 맵 파일(Memory Mapped File)을 사용한 가상 메모리를 사용한다

- 몽고디비의 데이터 구조는 데이터를 저장하는 Record , 인덱스를 저장하는 Bucket.

  - **Record** : BSON 객체를 저장하는 노드를 레코드로 정의
  - MongoDB는 BSON 객체를 데이터 저장 단위로 사용
  - BSON 객체의 이중연결리스트 (double linked list)구조로 구성
  - **Bucket** : 인덱스는 레코드에 저장된 데이터를 빠르게 찾기 위해 b-tree 형태로 저장된 노드 구조를 가짐
  - b-tree 노드를 버켓(Bucket)이라고 정의

- Extent

   

  : MongoDB는 대용량 데이터를 HDD에 쉽게 저장할 수 있는 단위로 레코드들을 그룹핑grouping한다.

  - 이를 익스텐드extent라고 한다
  - 익스텐드들을 이용하여 MongoDB는 HDD에 저장될 파일과 삭제된 레코드를 관리한다.
  - 자료 구조 관점에서 보면 연결되어 있는 레코드들의 헤더 역할을 수행하는 것

- 사용자가 하나의 데이터베이스를 만들었다면, MongoDB는 데이터베이스와 관련된 한 개의 **네임스페이스(DB Namespace)**를 만든다.

- DB Namespace는 **컬렉션 (collection Namespace)** 와 **Free Extent 리스트**(DB에서 삭제된 레코드 리스트를 가지고 있는 Extent 리스트)를 가진다.

- Collection Namespace는 primary index Namespace(B Tree)와 Record Extent list를 가진다.

- 만약 Primary index 이외에 추가로 필드를 가지고 인덱스를 더 생성하면 B Tree 형태의 Index Namespace를 하나 더 가진다.

- Write(insert/update)를 수행할 때, MongoDB는 해당 레코드가 가리키는 가상 메모리 주소 공간에 데이터를 적재한다.

- Read를 수행할 때, MongoDB는 해당 메모리 주소 공간에 할당된 데이터가 가상 메모리에 로딩되어 있는지 확인하고, 없다면 파일에서 내용을 읽어 가상 메모리에 적재한다.

- MongoDB는 백그라운드로 (주기적으로)가상 메모리에 적재된 데이터를 HDD에 최대 2GB 단위로 파일을 구성한 볼륨(volume)으로 HDD에 데이터를 flush한다.



### Record Structure

![mongo_record_structure](http://nicewoong.github.io/assets/mongo_record_structure.png)

- 헤더는 총 16바이트 크기
- 데이터는 도큐멘트를 BSON 객체 형태로 가진다.
  - Size : 헤더를 포함하고 있는 레코드의 크기, BSON 객체의 크기는 Size 필드에서 헤더 크기인 16을 빼면 된다.
  - Extent Offset : 해당 레코드가 속해 있는 익스텐트(Extent)의 Offset 주소
    - (MongoDB는 모든 위치를 DiskLoc이라는 구조체를 사용
    - volume 정보와 offset 정보 두 개를 가지는 구조체.
    - 여기서 말하는 offset은 DiskLoc의 offset 주소와 동일)
  - Next Offset : 다음 레코드를 가리키는 Offset 주소
  - bson object : 도큐멘트를 구성하는 BSON 객체
- 참고 : Document의 크기는 Field 명을 포함하므로 필드명을 길게하고 하면 데이터가 많아질 수록 Document 크기가 커진다



### Extent Structure

![mongo_extent_structure](http://nicewoong.github.io/assets/mongo_extent_structure.png)

- MongoDB의 Extent는 Record들의 그룹을 말한다.
- 헤더 4바이트와 데이터 172바이트로 구성 (총 178바이트의 크기)
  - Magic : 4바이트 헤더이며, 정수 781231 값을 가진다.
  - My Location: 해당 Extent가 저장된 위치 정보를 나타낸다.
  - Next Extent : 다음 Extent를 가리킴
  - Prev Extent : 이전 Extent를 가리킴
  - Namespace : 해당 Extent의 네임 스페이스
  - Size : 해당 익스텐드의 크기
  - First Record : 해당 Extent에 포함된 첫번째 레코드를 가리킴
  - Last Record : 해당 Extent에 포함된 마지막 레코드를 가리킴
- My Location, Next Extent, Prev Extent, First Record, Last Record
  - 들은 모두 8바이트 크기를 가지고 있다.
  - 64비트 컴퓨터의 주소는 64비트 크기를 가지고 있기 때문에 8바이트 크기를 가진다.
  - 그리고 size 필드가 4바이트로 규정되어 있다.
  - MSBMost Significant Bit 를 제외하면 2GB의 크기 까지 익스텐트의 크기를 지정할 수 있다.
- MongoDB는 로컬 스토리지에 데이터베이스 명으로 확장자가 숫자로 증분하는 형태의 파일을 가진다.
  - 예를 들어 test.1이라고 하면, test라는 데이터베이스의 첫번째 익스텐트를 의미한다.





## (2) WierdTiger 스토리지엔진 구조

------

![wiredtiger_internal](http://nicewoong.github.io/assets/wiredtiger_internal.jpg)

- MMAPv1 비교했을 때 대표적인 특징 중에 하나는 데이터 압축(Compression) 기능과 데이터 암호화(Encryption) 기능

- MMAPv1 에 비해서 WiredTiger 는 more general purpose 이다.

- MVCC (Multi-Version-Concurrency-Control) - document-level 에서 병렬성을 제공한다.

- ```
  B+ tree
  ```

   

  구조와

   

  ```
  LSM
  ```

   

  구조를 모두 지원한다. 하지만 mongodb 는 B+ Tree 만 지원한다.

  - 이는 read workloads 에 최적화 되어 있다.

- CheckSums 기능을 통해 시스템 장애 또는 저장 장치 장애로부터 발생하는 데이터 유실을 최소화할 수 있다. (File System Corrupt 상태 분석)

- Highly concurrent and vertically scalable

- Document level locking

- Allows for more tuning of storage engine than MMAP

- Compression

- On-line compaction

- Write-ahead transaction log for the journal

- 주의!

  - WiredTiger does **not** support in place updates
  - Journaling 기능을 제공하지 않는다.



#### Scalability

- 멀티코어 시스템에서 효율이 좋다.
- MMAPv1 는 CPU를 추가해도 성능이 향상되지 않는다.

#### Concurrency

- Document level 에서 locking 을 수행한다.
- 그래서 동시성이 훨씬 좋다.
- MMAPv1는 Collection level 에서 locking 을 수행했다.

#### Compression

- ```
  gzip
  ```

   

  과

   

  ```
  snappy
  ```

   

  방식의 Compression 을 제공한다.

  - `Snappy` : 기본 압축 기능이며 압축율이 좋고 오버헤드가 적게 발생한다.
  - `gzip` : Snappy 압축 방법에 비해 매우 높은 압축율을 제공되지만 CPU 오버헤드가 발생한다.

- Collection 의 사이즈가 더 작다. 저장공간을 효율적으로 사용가능.

- Index에 대해서도 압축(Compression) 기능을 제공

  - 디스크에서도, 메모리에서도 공간 효율 좋아짐.





## 참고

- [몽고디비 데이터 구조](http://mongodb.citsoft.net/)
- [mongodb internals (Slide Share )](https://www.slideshare.net/NorbertoLeite/mongodb-internals-55965341)
- [A Technical Introduction to WiredTiger(Slide Sahre)](https://www.slideshare.net/wiredtiger/mongo-db-worldwiredtiger?next_slideshow=1)
- [Inside MongoDB: the Internals of an Open-Source Database(Slide Share )](https://www.slideshare.net/mdirolf/inside-mongodb-the-internals-of-an-opensource-database)
- [Practical MongoDB(book)](https://books.google.co.kr/books?id=7iI3CwAAQBAJ&pg=PA166&lpg=PA166&dq=mongodb+b+tree&source=bl&ots=cLtkVTWESk&sig=AqOe913XuVKGOf30YzqpF2QNIBQ&hl=ko&sa=X&ved=0ahUKEwj2vran6rzXAhUMy7wKHVS_AFk4ChDoAQg9MAM#v=onepage&q=mongodb b tree&f=false)
- [WiredTiger 와 MMAPv1 비교](https://www.vividcortex.com/blog/why-wiredtiger-is-the-default-mongodb-storage-engine)
- [wiredtiger](http://learnmongodbthehardway.com/schema/wiredtiger/)
- [MongoDB의 새로운 WiredTiger StorageEngine](http://www.dbguide.net/knowledge.db?cmd=specialist_view&boardUid=189579&boardConfigUid=91&boardStep=0&categoryUid=)