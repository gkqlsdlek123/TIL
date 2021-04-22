## MongoDB 튜토리얼 따라해보기



### 설치

- MongoDB 최신 버전 : 3.4, Community Edition 을 설치하는 과정입니다.
- [공식 사이트 설치 메뉴얼 (클릭)](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)
- 먼저 package management system 을 위한 public key 를 import합니다.

```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

- 아래와 같은 결과가 출력됩니다.

  ```
     Executing: /tmp/tmp.PEgHdGAUI9/gpg.1.sh --keyserver
     hkp://keyserver.ubuntu.com:80
     --recv
     0C49F3730359A14518585931BC711F9BA15703C6
     gpg: requesting key A15703C6 from hkp server keyserver.ubuntu.com 
     gpg: key A15703C6: public key "MongoDB 3.4 Release Signing Key <packaging@mongodb.com>" imported
     gpg: Total number processed: 1
     gpg:               imported: 1  (RSA: 1)
  ```

- MongoDB list file을 생성합니다. 우분투 버전:

   

  ```
  16.04
  ```

  에 맞는 명력어를 사용합니다.

  - `$ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

- Reload local package database

  - `$ sudo apt-get update`

- MongoDB packages를 설치해봅시다.

  - `$ sudo apt-get install -y mongodb-org`

#### (참고) Public key는 왜?

- mongodb 설치중 public key import 설명이 아래와 같이 나와있다.

  - Import the public key used by the package management system.
  - The Ubuntu package management tools (i.e. dpkg and apt) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys. Issue the following command to import the MongoDB public GPG Key:
  - Ubuntu 패키지 관리 도구 (예 : dpkg 및 apt)는 배포자가 GPG 키를 사용하여 패키지에 서명하도록 요구함으로써 패키지 일관성 및 신뢰성을 보장합니다.
  - `$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`
  - 공개키를 서버에 업로드 하는 것

- GPG key

  - mongodb public GPG key 를 import 하라며 아래와 같이 pubic GPG key 를 제공한다.

    ```
     -----BEGIN PGP PUBLIC KEY BLOCK-----
     Version: GnuPG v1.4.11 (GNU/Linux)
     mQINBFaUNhsBEACkTlpL9xCrlirl77tahFzzd9ccTc5wP+M3oob18GIaMYKicjbR
     h6J6ytCiXCkl65zYKvQdLkt8qlkBVc5DxGeJvD41IY3NzGPz+BZ9pFFBndAE+JEP
     ng0ULLxzUDmWXIoukdHqf92BSizTFd2A8v+YGuwOkNBdPi/BHkwiViAaAKDZm/4k
     9LZeOF0v7gZF89QD75NrSCKo5SGFRb8Cxi4KR4cS/jPuQVjd+B9fWkc74BUWE91t
     3R87Uypd+1qnmoN6cOssLZ4s8n/cyOCkVphGmk1tDDhbEsI4knOqtPXaBHiC4lVI
     ghpTHEDUuDfbQ7scySae8/YItTC/vVGngiJmZSfZU5AvVspe6rfkHQHqZs3gYMqj
     XPl7acviEAZ7OiMp9diq6Kgp+xLRvRGL+jtUjLkP5O4gJlnxCm7YWrYfYA/vHULD
     MyIGSBzuESGxL+Ygz+Dc0Aim9NPM5KhpV5FoAXNt50cn6n1adIwbUciRY0zBXKAI
     Vj6D+j3e0ozsO+GGEpmQFAIo1h7CEn8VV61WaLz2F60LKR8d/DEMZ7SY8uznbzkm
     TJCeCp/pTnPeGwkyJmJ78LAaKw2tSCeEAfRlnzPeQeanOnEX/wnAjHHAHewvGgQe
     GW1QkEdy8zNmfODDf9wqknBShaFRHAOAQFEgBAkYHuT4SgHqW8TVDtF3CQARAQAB
     tDdNb25nb0RCIDMuNCBSZWxlYXNlIFNpZ25pbmcgS2V5IDxwYWNrYWdpbmdAbW9u
     Z29kYi5jb20+iQI+BBMBAgAoBQJWlDYbAhsDBQkDwmcABgsJCAcDAgYVCAIJCgsE
     FgIDAQIeAQIXgAAKCRC8cR+boVcDxmtEEACSjnZcwcozGYS/8peH2P8yPxD2mXVQ
     AJ8Pss+YBo8hpRaiA7BEY+FFthbSYEX8XRR/Bg9HjDk9CNXc221I0WcTRv3Sb718
     QutRd4ppdGtusgTHjUdYNDzctExU90vtJRvwI2oiz2YA8dM7mtTzUFpR4IQGopB4
     PmjEls6hkebTjjSaO9UmcLyip+S+rTZ9c8UQvBH7rNoe4QacmGi/l/uUo/q4J7nE
     jtjpsemUK7LWY7YtB21F/hH3OrQkgQAoVv2q2xSaiLJeWsr33jgd4o4/d3QN1t/P
     GkNIOEBdO/hM8uOj+hGD+tDphHzd9jGjALqV6lC2k9zNXyAFnTUwp0NL74hODv6z
     daihKu4fTRU7S0eYSGc2sQDPiiQF5YkxAHqADnPmR2ZpBVVtbUNB31BDOYjTzRwq
     tkLKRCgI29Kgut0Uhvq+/Hx+0485ndgzcqeaLhslUagZy1bXN3sDW4QYN2tPvP+P
     2JDtGydsYGZCWA0FBRFdsSbruBSK/BkEpGhq97bE9vclfVchb989A47lgErusw5C
     xtLxUGPmVc2dYmHJLUkgHszdcTLHwy8/arYMehG7RVzAEG55AueLsc9B0vSI0E6r
     lvalHgoCttCynEzM4Ol1rcG9XtlCyKk4AeimYLE/cxlckDoIVVwrFXrRrhB41Asw
     rP4l4xtk+nWHpg==
     =F42J
     -----END PGP PUBLIC KEY BLOCK-----
    ```

- GPG key란?

  - Gnu Privacy Guard
  - GPG(또는 PGP라고도 불려짐)는 강력한 암호화 프로그램
  - GPG는 받는 사람과 보내는 사람 이 둘만 해독하고 암호화 할 수 있음
  - GPG는 RSA 암호 기술을 이용
    - RSA는 암호(이하 키)가 2개입니다. 어떤 키로 암호화하면 다른 키로만 복호화할 수 있습니다.





### 실행

- - 정상적으로 설치가 되었는지 MongoDB를 실행시켜봅시다.

    `$ sudo service mongod start`

- MongoDB가 잘 실행되었는지 확인을 해봅시다.

- - 다음 경로에 있는 /var/log/mongodb/mongod.log 파일을 열어서 로그를 확인해보는 거에요. `cat`명령어를 이용합시다.

    `$ cat /var/log/mongodb/mongod.log`

- - 해당 로그 파일을 보면 가장 아래쪽 최신 로그가 아래와 같이 출력되어 있으면 정상 실행된 상태에요.

    `2017-08-30T17:19:17.727+0900 I NETWORK [thread1] waiting for connections on port 27017`

- 기본 포트는 `27017` 이며, 방화벽을 사용하시는분들은 해당 포트를 Open 해주세요.

- 포트는 `/etc/mongod.conf` 에서 변경 가능합니다.

- - MongoDB를 stop 해봅시다.

    `$ sudo service mongod stop`

- 로그파일을 확인해보면 아래와 같은 로그가 추가되어있는 것을 확인할 수 있습니다.

  ```
    2017-08-30T17:26:55.910+0900 I CONTROL  [signalProcessingThread] got signal 15 (Terminated), will terminate after current cmd ends
    2017-08-30T17:26:55.910+0900 I NETWORK  [signalProcessingThread] shutdown: going to close listening sockets...
    2017-08-30T17:26:55.911+0900 I NETWORK  [signalProcessingThread] closing listening socket: 7
    2017-08-30T17:26:55.911+0900 I NETWORK  [signalProcessingThread] closing listening socket: 8
    2017-08-30T17:26:55.911+0900 I NETWORK  [signalProcessingThread] removing socket file: /tmp/mongodb-27017.sock
    2017-08-30T17:26:55.911+0900 I NETWORK  [signalProcessingThread] shutdown: going to flush diaglog...
    2017-08-30T17:26:55.911+0900 I FTDC     [signalProcessingThread] Shutting down full-time diagnostic data capture
    2017-08-30T17:26:55.914+0900 I STORAGE  [signalProcessingThread] WiredTigerKVEngine shutting down
    2017-08-30T17:26:56.442+0900 I STORAGE  [signalProcessingThread] shutdown: removing fs lock...
    2017-08-30T17:26:56.443+0900 I CONTROL  [signalProcessingThread] now exiting
    2017-08-30T17:26:56.443+0900 I CONTROL  [signalProcessingThread] shutting down with code:0
  ```

- - MongoDB를 restart해봅시다.

    `$ sudo service mongod restart`

- 그러면 아래 로그가 가장 아래쪽에 출력되어 있습니다.

  ```
    2017-08-30T17:54:38.596+0900 I NETWORK  [thread1] waiting for connections on port 27017
  ```





### 기본 조작 1



#### Client에 접속하기

- `$ mongo `라고 입력하면 몽고디비 명령어를 사용할 수 있는 창으로 진입합니다.



#### DB생성하기

- `> use DATABASE_NAME` 명령어를 통하여 Database를 생성 할 수 있습니다

- 생성되어 있는 DB명이라면 해당 DB를 사용하는 것

- 존재 하지 않는 DB name 이라면 새로운 DB를 생성하고 사용하는 것

- - 현재 사용중인 데이터베이스를 확인

    `> db `명령어를 입력

- - 내가 만든 데이터베이스 리스트들을 확인

    `show dbs `입력

- 아래와 같이 출력됨. 내가 방금 만든 test_db가 존재하지 않음. (최소 하나의 doc 이 저장되어 있어야 나타납니다. )

  ```
    admin  0.000GB
    local  0.000GB
  ```

- 아래와 같이 test 데이터를 입력 후 다시 확인해봅시다.

  ```
    > db.test_collection.insert({"test_name":"woongje", "test_numebr":1234})
    WriteResult({ "nInserted" : 1 })
      
    > show dbs
    admin    0.000GB
    local    0.000GB
    test_db  0.000GB
  ```



#### DB제거하기

- 제거하고자 하는 DB로 진입하고나서 삭제를 해야함.

  ```
    > use test_db
    switched to db test_db
    > db.dropDatabase();
    { "dropped" : "test_db", "ok" : 1 }
  ```

- 위와 같이 test_db가 drop되었다는 메시지를 확인할 수 있다.

  #### Collection 생성하기

- `> db.createCollection("COLLECTION_NAME", OPTIONS)` 명령

  ```
    > db.createCollection("test_collection")
    { "ok" : 1 }
  ```

- 옵션과 함께 생성하기

  ```
    > db.createCollection("People", {
    ... capped: false,
    ... max : 10000 })
    결과
    { "ok" : 1 }
  ```

- 명시적으로 생성하지 않더라도 document를 삽입하면서 collection명을 넣어주면 자동으로 생성됩니다.

  ```
    > db.Buildings.insert({"name" : "63Building"})
    WriteResult({ "nInserted" : 1 })
  ```

- 만들어진 Collection 목록을 확인해봅시다. `> show collections`

  ```
    > show collections 
    People
    test_collection
  ```



#### Collection 제거하기

- `db.C_NAME.drop()` 명령어를 사용합니다.

- 콜렉션을 확인해보고 - 삭제하고 - 다시 확인해봅시다.

  ```
   > show collections
   Buildings
   People
   test_collection
   > db.test_collection.drop()
   true
   test_collection이라는 이름의 Collection이 drop 되었음을 확인할 수 있습니다. 
   > show collections
   Buildings
   People
  ```



#### Collection에 Dcoument 삽입

- [(참고) - CRUD 공식가이드라인](https://docs.mongodb.com/manual/crud/)

- 다수 데이터 삽입하기

- 한 개의 Doc 넣는 것과 여러 개의 Doc 넣는 것이 있는데

  - `db.collection.insertOne()` New in version 3.2
  - `db.collection.insertMany()` New in version 3.2

- 그냥 `db.collection.insert()` <- 이 메서드 하나로 다 되더라구요.

- 아래와 같이 배열 형태로 Doc 을 여러개 삽입가능.

  ```
   > db.Students.insert([{"name":"han",  "age":26}, {"name":"Kim", "age":26}]) 
   BulkWriteResult({
   	"writeErrors" : [ ],
   	"writeConcernErrors" : [ ],
   	"nInserted" : 2,
   	"nUpserted" : 0,
   	"nMatched" : 0,
   	"nModified" : 0,
   	"nRemoved" : 0,
   	"upserted" : [ ]
   })
  ```

- **Sample data**

  ```
    db.inventory.insertMany( [
      { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
      { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
      { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
      { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
      { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
      { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
      { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
      { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
    ]);
     
   db.inventory.insertMany( [
      { item: "b", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
      { item: "c", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "d", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
      { item: "e", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
      { item: "f", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
      { item: "g", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
      { item: "h", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
      { item: "i", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
      { item: "j", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "k pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" },
      { item: "l", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
      { item: "m", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "n", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
      { item: "o", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
      { item: "p", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
      { item: "q", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
      { item: "r", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
      { item: "s", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
      { item: "t", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "u pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" },
      { item: "v", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
      { item: "w", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" },
      { item: "x", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
      { item: "y", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
      { item: "z", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
      { item: "11", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" } 
   ]);
  ```



#### Collection의 Documents 모두 출력하기

- `db.collection.find() `명령어를 이용해서 list를 출력할 수 있습니다.

  ```
    > db.Buildings.find()
    { "_id" : ObjectId("59a79eb76e01d3e60d24a3a5"), "name" : "63Building" }
    { "_id" : ObjectId("59a7b9dfcde2cac42d57c60e") }
    { "_id" : ObjectId("59a7b9dfcde2cac42d57c60f"), "name" : "Happy Building", "Height" : 135 }
    { "_id" : ObjectId("59a7b9dfcde2cac42d57c610"), "name" : "Greddy Building" }
  ```



#### Collections 에서 데이터 삭제

- `db.collection.deleteOne() `조건에 맞는 document 하나 삭제

- `db.collection.deleteMany() `조건에 맞는 document 모두 삭제

- 아래와 같이 status 가 “A” 값을 가지는 것을 모두 삭제합니다.

  ```
    > db.inventory.deleteMany({status:"A"})
    { "acknowledged" : true, "deletedCount" : 6 }
  ```



#### Collections 에서 Data 조회하기

- `db.collection.find( { DOCUMENT } )`

- 아래와 같이 stauts 값이 P 인것을 모두 조회합니다.

  ```
    > db.inventory.find({status:"P"})
  ```

- Embeded doc 에 대한 쿼리는 어떻게 할까요? ** . 연산자를 이용하면 됩니다.

  ```
    > db.inventory.find({"size.h":28})
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "item" : "canvas", "qty" : 100, "size" : { "h" : 28, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
  ```





### Query 문 사용 하기



#### 값 비교 옵션들

- `$eq` (equals) 주어진 값과 일치하는 값
- `$gt` (greater than) 주어진 값보다 큰 값
- `$gte` (greather than or equals) 주어진 값보다 크거나 같은 값
- `$lt` (less than) 주어진 값보다 작은 값
- `$lte` (less than or equals) 주어진 값보다 작거나 같은 값
- `$ne` (not equal) 주어진 값과 일치하지 않는 값
- `$in` 주어진 배열 안에 속하는 값
- `$nin` 주어빈 배열 안에 속하지 않는 값



#### 옵션 사용 예시

- qty 값이 40보다 크고, 60보다 작은 것을 조회

  ```
    > db.inventory.find({qty: {$gt:40, $lt:60}})
    { "_id" : ObjectId("59a8029acf8614e7881b476a"), "item" : "notebook", "qty" : 50, "size" : { "h" : 8.5, "w" : 11, "uom" : "in" }, "status" : "P" }
    { "_id" : ObjectId("59a8029acf8614e7881b476d"), "item" : "postcard", "qty" : 45, "size" : { "h" : 10, "w" : 15.25, "uom" : "cm" }, "status" : "A" }
  ```

- item의 문자열이 [ “”, “”] 배열 속에 속하는 문자열인 것 뽑아내기 => $in 옵션을 사용해서 아래와 같이 쓰면 돼요.

  ```
    > db.inventory.find({item:{$in:["canvas", "mat", "father"]}})
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "item" : "canvas", "qty" : 100, "size" : { "h" : 28, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
    { "_id" : ObjectId("59a8029acf8614e7881b4768"), "item" : "mat", "qty" : 85, "size" : { "h" : 27.9, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
  ```



#### 논리연산자 옵션도 있습니다.

- - 논리연산자 사용 예시

    `{$or: [{ __CONDITION1__ }, { __CONDITION2__}]}`

- or 연산자를 이용해서 item 이 “canvas” 이거나 qty 값이 55보다 작은 것을 조회해보겠습니다.

  ```
    > db.inventory.find({$or: [{item:"canvas"}, {qty:{$lt:55}}]})
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "item" : "canvas", "qty" : 100, "size" : { "h" : 28, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
    { "_id" : ObjectId("59a8029acf8614e7881b4767"), "item" : "journal", "qty" : 25, "size" : { "h" : 14, "w" : 21, "uom" : "cm" }, "status" : "A" }
    { "_id" : ObjectId("59a8029acf8614e7881b4769"), "item" : "mousepad", "qty" : 25, "size" : { "h" : 19, "w" : 22.85, "uom" : "cm" }, "status" : "P" }
    { "_id" : ObjectId("59a8029acf8614e7881b476a"), "item" : "notebook", "qty" : 50, "size" : { "h" : 8.5, "w" : 11, "uom" : "in" }, "status" : "P" }
    { "_id" : ObjectId("59a8029acf8614e7881b476d"), "item" : "postcard", "qty" : 45, "size" : { "h" : 10, "w" : 15.25, "uom" : "cm" }, "status" : "A" }
  ```



#### Projection Option

- - 조회 결과에서 선택적으로 element만 나타낼 수 있습니다.

    `find({}, {__PROJECTIONS__, , ... })``> db.inventory.find({}, {"item":true, "qty":true, _id:false}) { "item" : "canvas", "qty" : 100 } { "item" : "journal", "qty" : 25 } { "item" : "mat", "qty" : 85 } { "item" : "mousepad", "qty" : 25 } { "item" : "notebook", "qty" : 50 } `



#### 각종 덧붙이는 메서드

- Sort

  - `.sort()` 메서드를 사용

  - 오름차순은 1, 내림차순은 -1 로 해서

    ```
    > db.inventory.find({},{status:true, qty:true, _id:false}).'''sort({"status":1, qty:-1})'''
    { "qty" : 100, "status" : "A" }
    { "qty" : 95, "status" : "A" }
    { "qty" : 85, "status" : "A" }
    { "qty" : 80, "status" : "A" }
    { "qty" : 45, "status" : "A" }
    { "qty" : 25, "status" : "A" }
    { "qty" : 100, "status" : "D" }
    { "qty" : 75, "status" : "D" }
    { "qty" : 50, "status" : "P" }
    { "qty" : 25, "status" : "P" }
    ```

- limit

  - 데이터 개수를 제한할 때 `.limit(5) `메서드를 사용합니다.

    ```
    > db.inventory.find().limit(3)
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "item" : "canvas", "qty" : 100, "size" : { "h" : 28, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
    { "_id" : ObjectId("59a8029acf8614e7881b4767"), "item" : "journal", "qty" : 25, "size" : { "h" : 14, "w" : 21, "uom" : "cm" }, "status" : "A" }
    { "_id" : ObjectId("59a8029acf8614e7881b4768"), "item" : "mat", "qty" : 85, "size" : { "h" : 27.9, "w" : 35.5, "uom" : "cm" }, "status" : "A" }
    ```

- skip

  - 처음 n 개의 doc 을 생략합니다. `db.inventory.find().skip(3)`





### DATA UPDATE

- 두 가지 메서드가 있습니다.

  - `updateOne()`
  - `updateMany()`

- 찾아서 Doc을 덮어쓰기

  ```
    > db.inventory.update({"status":"A"}, {name:"Kim mijeong"})
    WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
    '''그 결과 아래와 같이 Dco이 name key 만 가지도록 바꼈습니다.  '''
    > db.inventory.find()
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "name" : "Kim mijeong" }
  ```

- - 찾아서 Doc의 특정 Field 변경하기

    `$set : { "key" : "value"} 옵션을 사용합니다.``> db.inventory.update({name : "Kim mijeong"}, {$set : {age: 28}})  WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 }) '''결과를 확인해봅시다.''' > db.inventory.find({"name": {$in : ["Kim mijeong","mijeo"] }}) { "_id" : ObjectId("59a8029acf8614e7881b4766"), "name" : "Kim mijeong", "age" : 28 } `

- 찾아서 Doc의 특정 Field 누락시키기

  - `{$unset : {age: 1}} `에서 1은 true 라는 의미로 넣어줍니다.

    ```
    > db.inventory.update({"name":"Kim mijeong"}, '''{$unset : {age: 1}}''') 
    WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
    '''결과를 확인해봅시다 '''
    > db.inventory.find({"name": {$in : ["Kim mijeong","mijeo"] }})
    { "_id" : ObjectId("59a8029acf8614e7881b4766"), "name" : "Kim mijeong" }
    ```





## 참고

- [Getting started Guide (official)](https://docs.mongodb.com/manual/core/databases-and-collections/)
- [튜토리얼 정리 블로그(한글)](https://velopert.com/mongodb-tutorial-list)
- [튜토리얼 강의(한글)](https://www.youtube.com/watch?v=eh1Lz6imsBM&list=PL9FpF_z-xR_GMujql3S_XGV2SpdfDBkeC&index=32)