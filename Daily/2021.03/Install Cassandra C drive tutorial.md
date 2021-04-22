## Install Cassandra C drive tutorial

- [공식 설명 문서(v2.8) - DataStax C/C++ Driver for Apache Cassandra](https://docs.datastax.com/en/developer/cpp-driver/2.8/topics/)
- [cpp-drivere 우분투 v2.8 다운로드 서버](http://downloads.datastax.com/cpp-driver/ubuntu/16.04/cassandra/v2.8.1/)
- [cpp-driver 우분투 디펜던시 다운로드 서버(최신거)](http://downloads.datastax.com/cpp-driver/ubuntu/16.04/dependencies/)



- `cpp-driver` 다운로드 서버와 `dependancies` 다운로드 서버에 접속하면 `.deb` 파일이 각 세 개씩 올라가 있다.





### `.deb` 파일이란?

- `.deb`은 데비안의 소프트웨어 패키지 포맷의 확장자
- 패키지 관리 시스템
- 데비안 소프트웨어 포맷의 `바이너리 패키지`에서 가장 자주 사용되는 파일 이름이다.
- 데비안 패키지는 데비안 기반의 `GNU`/리눅스 배포판 (`우분투` 등)에서 동작한다.

#### deb 파일 설치

```
    # dpkg -i [deb파일]
```

#### 패키지 삭제

```
    # dpkg -r [패키지 이름]
```

#### 설치된 패키지 검색

```
    # dpkg -l
```





## .deb 파일 모두 다운받기.

- ```
  wget
  ```

   

  명령어를 이용해서 다운로드 서버에서 단운로드 한다.

  - url 은 위 다운로드 서버에서 해당 파일을 오른쪽 클릭해서 링크를 얻어왔다.

    ```
    $ sudo wget http://downloads.datastax.com/cpp-driver/ubuntu/16.04/cassandra/v2.8.1/cassandra-cpp-driver_2.8.1-1_amd64.deb
    ```

- Cassandra-cpp-driver패키지와 cpp-driver-dependancies 에 `...-dbg` 랑 `...-dev` 파일 등을 모두 다운로드 한다.

- 그러면 총 여섯개의 파일이 다운로드된다.

  ```
    $ ls -l
    -rw-r--r-- 1 root root  419554 Feb 15 02:48 cassandra-cpp-driver_2.8.1-1_amd64.deb
    -rw-r--r-- 1 root root 8651010 Feb 15 02:48 cassandra-cpp-driver-dbg_2.8.1-1_amd64.deb
    -rw-r--r-- 1 root root  535830 Feb 15 02:48 cassandra-cpp-driver-dev_2.8.1-1_amd64.deb
    -rw-r--r-- 1 root root   60990 Feb 15 02:48 libuv_1.18.0-1_amd64.deb
    -rw-r--r-- 1 root root  197746 Feb 15 02:48 libuv-dbg_1.18.0-1_amd64.deb
    -rw-r--r-- 1 root root   80032 Feb 15 02:48 libuv-dev_1.18.0-1_amd64.deb
  ```





## 다운로드 받은 .deb 파일 설치하기

- `dpkg`를 통해 설치를 시도했는데 아래와 같은 오류가 발생

  ```
    # dpkg -i cassandra-cpp-driver_2.8.1-1_amd64.deb 
    Selecting previously unselected package cassandra-cpp-driver.
    (Reading database ... 5073 files and directories currently installed.)
    Preparing to unpack cassandra-cpp-driver_2.8.1-1_amd64.deb ...
    Unpacking cassandra-cpp-driver (2.8.1-1) ...
    dpkg: dependency problems prevent configuration of cassandra-cpp-driver:
     cassandra-cpp-driver depends on libuv; however:
      Package libuv is not installed.
  ```

- libuv 가 필요하다고 한다.

- 그렇다. libuv 파일을 `dpkg`를 통해 설치하고나서 cpp-driver를 설치하니깐 에러 없이 순식간에 끝이났다.

  ```
    # dpkg -i cassandra-cpp-driver_2.8.1-1_amd64.deb 
    (Reading database ... 5083 files and directories currently installed.)
    Preparing to unpack cassandra-cpp-driver_2.8.1-1_amd64.deb ...
    Unpacking cassandra-cpp-driver (2.8.1-1) over (2.8.1-1) ...
    Setting up cassandra-cpp-driver (2.8.1-1) ...
    Processing triggers for libc-bin (2.23-0ubuntu10) ...
  ```





## 테스트 코드 작성해보기.

- 샘플코드는 아래 링크를 참조하십시오.

  - [샘플코드바로가기](https://github.com/nicewoong/cassandra_c_driver/blob/master/sample_modularized.c)

- `test.c `를 만들어서 `#include <cassandra.h>` 를 포함시켜서 컴파일 해봤다.

  ```
            # gcc test.c -o test 
  ```

- 하지만 test 코드에서 사용된 함수들을 못찾겠다고 컴파일 에러…;

  ```
    # gcc test.c
    /tmp/ccBWYYkU.o: In function `main':
    test.c:(.text+0xe): undefined reference to `cass_cluster_new'
    test.c:(.text+0x1c): undefined reference to `cass_session_new'
    test.c:(.text+0x31): undefined reference to `cass_cluster_set_contact_points'
    test.c:(.text+0x44): undefined reference to `cass_session_connect'
    test.c:(.text+0x54): undefined reference to `cass_future_error_code'
    test.c:(.text+0x61): undefined reference to `cass_error_desc'
    test.c:(.text+0x7f): undefined reference to `cass_future_free'
    test.c:(.text+0x8b): undefined reference to `cass_session_free'
    test.c:(.text+0x97): undefined reference to `cass_cluster_free'
  ```

- 경로를 지정해줘야하는건가?

- 그래서 아래 명령어로 `cassandra-cpp-driver~.deb` 파일을 설치했을 때 만들어낸 파일 목록을 펼쳐봤다.

  ```
    # dpkg -c cassandra-cpp-driver_2.8.1-1_amd64.deb 
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/share/
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/share/doc/
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/share/doc/cassandra-cpp-driver/
    -rw-r--r-- root/root       845 2018-02-14 16:11 ./usr/share/doc/cassandra-cpp-driver/copyright
    -rw-r--r-- root/root       212 2018-02-14 16:11 ./usr/share/doc/cassandra-cpp-driver/changelog.Debian.gz
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/lib/
    drwxr-xr-x root/root         0 2018-02-14 16:14 ./usr/lib/x86_64-linux-gnu/
    -rw-r--r-- root/root   1535928 2018-02-14 16:14 ./usr/lib/x86_64-linux-gnu/libcassandra.so.2.8.1
    lrwxrwxrwx root/root         0 2018-02-14 16:14 ./usr/lib/x86_64-linux-gnu/libcassandra.so.2 -> libcassandra.so.2.8.1 
  ```

- 검색하다가… 아래와 같이 입력했더니 컴파일은 완료 되었다.

  ```
    # gcc test.c -o test -L/usr/lib/ -lcassandra
  ```

- test 실행파일을 실행하면 아래와 같이 출력이 된다. 근데 connect result에 아무것도 출력 안 된다..;

  ```
    # ./test 
    Connect result: 
  ```

- 그렇다. 이것은 성공한 것이었다. 쿼리하는 코드를 추가해보니 잘 적용이 됐다 .하하. 굿~

- Installation 설명에 아래와 같이 적혀있는데, 무슨 뜻이지!!!????

> Note: CentOS and Ubuntu use the version of OpenSSL provided with the distribution.