# Docker 사용법 (Linux에서)

### Reference

- [초보를 위한 도커 안내서 - 설치하고 컨테이너 실행하기](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)
- [감성 프로그래밍 - [Docker\] Docker 시작하기](http://programmingsummaries.tistory.com/391)
- [도커 튜토리얼 - 깐김에 배포까지](http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/)
- [도커 기본 사용법](http://pyrasis.com/Docker/Docker-HOWTO#search)

를 보고 직접 따라해본 것을 정리한 내용입니다.

- [도커란 무엇인가 - 조영규의 블로그](http://dev.youngkyu.kr/32) 를 보고 개념을 이해하기 쉬웠습니다.

### Docker 다운받기

```
curl -s https://get.docker.com/ | sudo sh
```

### Docker 사용 권한 주기

- docker 는 기본적으로 root 권한이 필요로 되어있다고 함.
- sudo 명령어 없이 docker를 이요하고 싶다면 아래 커맨드로 사용자에게 권한을 줍시다.

```
sudo usermod -aG docker your-user
```

- 그리고나서

   

  ```
  docker version
  ```

   

  을 입력하면 아래와 같은 결과를 볼 수 있음.

   

  버전확인

  하기

  - 버전정보 확인을 통해서 설치가 제대로 되었는지 판단가능 :)

```
~$ docker version
Client:
 Version:      17.09.0-ce
 API version:  1.32
 Go version:   go1.8.3
 Git commit:   afdb6d4
 Built:        Tue Sep 26 22:42:18 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.09.0-ce
 API version:  1.32 (minimum version 1.12)
 Go version:   go1.8.3
 Git commit:   afdb6d4
 Built:        Tue Sep 26 22:40:56 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

## 컨테이너사용

### 옵션

| **옵션** | **설명**                                               |
| :------- | :----------------------------------------------------- |
| -d       | detached mode 흔히 말하는 백그라운드 모드              |
| -p       | 호스트와 컨테이너의 포트를 연결 (포워딩)               |
| -v       | 호스트와 컨테이너의 디렉토리를 연결 (마운트)           |
| -e       | 컨테이너 내에서 사용할 환경변수 설정                   |
| –name    | 컨테이너 이름 설정                                     |
| –rm      | 프로세스 종료시 컨테이너 자동 제거                     |
| -it      | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| –link    | 컨테이너 연결 [컨테이너명:별칭]                        |

출처 : [초보를 위한 도커 안내서 - 설치하고 컨테이너 실행하기](https://www.gitbook.com/book/nicewoong/redis/edit#)

### 기본 Operation

- **이미지 다운받기 **

```
#우분투 이미지 다운받기 (pull)

~$ docker pull ubuntu:latest
# 아래와 같이 다운받는 과정이 나타남. 
latest: Pulling from library/ubuntu
9fb6c798fa41: Downloading [=============================>                     ]     28MB/47.54MB
3b61febd4aef: Download complete 
9d99b9777eb0: Verifying Checksum 
d010c8cf75d7: Download complete 
7fac07fb303e: Download complete 
```

- **다운 받은 docker image 확인하기**

```
~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              b6dddb991dfa        2 weeks ago         107MB
ubuntu              16.04               2d696327ab2e        2 weeks ago         122MB
ubuntu              xenial              2d696327ab2e        2 weeks ago         122MB
centos              latest              196e0ce0c9fb        3 weeks ago         197MB
ubuntu              trusty              dea1945146b9        3 weeks ago         188MB
```

- 이미지를 컨테이너로 실행한 뒤 bash shell 열기
  - -i 옵션 : interactive
  - -t 옵션 : tty
  - –name ubuntu01 : 해당 컨테이너 이름을 ubuntu01 이라 지정
  - /bin/bash : 해당 컨테이너의 bash shell 실행
  - **bash shell 에서 exit 하면 컨테이너가 자동으로 stop 됨.**

```
~$ docker run -i -t --name ubuntu01 ubuntu /bin/bash
#ubuntu 자리에 image id 를 입력해도 됨 

#아래와 같이 새로운 우분투 쉘로 진입한 것을 확인할 수 있음.
root@0a012fad0c3d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

- **exit 하고 빠져나와서 다시 ubuntu01 시작할때** `docker restart ubuntu01`
- **그리고 다시 해당 컨테이너에 접속하기**

```
~$ docker attach ubuntu01

# 새로 진입한 ubuntu01 에서 ls 명령 실행해보기
root@0a012fad0c3d:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

- **컨테이너 중지하기**

```
~$ docker stop 5555b7dd1385
5555b7dd1385
```

- **모든 컨테이너 중지하기**

```
~$ docker stop $(docker ps -a -q)
5555b7dd1385
b318cd2b8377
6c9e33af1272
a9ac6712167f
bb5b70c94b85
2ab2195d8895
5e53723c44cc
d4bdcfc7f373
b76f537a4fd6
```

- **컨테이너 재부팅하기**

```
~$ docker restart 5555b7dd1385
5555b7dd1385

#ps 명령어로 확인해보기
wjhan@triton8:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
5555b7dd1385        redis               "docker-entrypoint..."   12 hours ago        Up 5 seconds        6379/tcp            sad_lichterman
```

- **컨테이너 확인하기 (-a 옵션으로 종료된 컨테이너까지 확인하기)**

```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                    PORTS               NAMES
5555b7dd1385        redis               "docker-entrypoint..."   11 hours ago        Up 11 hours               6379/tcp            sad_lichterman
b318cd2b8377        redis               "docker-entrypoint..."   11 hours ago        Exited (0) 11 hours ago                       mystifying_yalow
6c9e33af1272        redis               "docker-entrypoint..."   13 hours ago        Exited (0) 12 hours ago                       hopeful_joliot
479266e88816        ubuntu:16.04        "/bin/bash"              18 hours ago        Exited (0) 18 hours ago                       reverent_wright
a9ac6712167f        centos              "/bin/bash"              9 days ago          Exited (127) 9 days ago                       gallant_mirzakhani
bb5b70c94b85        ubuntu:xenial       "/bin/bash"              9 days ago          Exited (0) 9 days ago                         vigorous_williams
2ab2195d8895        ubuntu:xenial       "/bin/bash"              9 days ago          Exited (0) 9 days ago                         modest_mccarthy
5e53723c44cc        ubuntu:xenial       "/bin/bash"              9 days ago          Exited (0) 9 days ago                         clever_visvesvaraya
d4bdcfc7f373        ubuntu:xenial       "/bin/bash"              9 days ago          Exited (0) 9 days ago                         sleepy_cray
b76f537a4fd6        ubuntu:xenial       "/bin/bash"              9 days ago          Exited (0) 9 days ago                         peaceful_haibt
```

- **컨테이너 삭제하기**

```
~$ docker rm 479266e88816
479266e88816
```

- **모든 컨테이너 삭제**

```
~$ docker rm $(docker ps -a -q)
5555b7dd1385
b318cd2b8377
6c9e33af1272
a9ac6712167f
bb5b70c94b85
2ab2195d8895
5e53723c44cc
d4bdcfc7f373
b76f537a4fd6
```

- **이미지 삭제하기**

```
docker rmi 이미지이름:이미지태그
```

- **모든 이미지 삭제하기**

```
~$ docker rmi $(docker images -q)
Untagged: redis:latest
Untagged: redis@sha256:ebb396dc3ac00e8eb4a64c1c022ef41ef16801f31ff98b16916a77fdc7252e67
Deleted: sha256:b6dddb991dfa5f8e49b00d2d4ff1e46e6593101ace99cca0febf287cadc4febf
Deleted: sha256:de0975f9263db0f0d446f5ed2789ec8316a1b81954215815bec6b1813545094a
Deleted: sha256:f4bca552ddb640965abb7cc6457c88960ed7e9b27e9ee4ae4e79307b9abbf015
Deleted: sha256:60087bf380ab081eba07e40965078d82b3f759910318e0cfce118b3f46b5a1d9
Deleted: sha256:c5bc4c295a15c2ff31c9edb3ecdcc18b1271f3fc7e544f64cf5ffb99c07fb555
Deleted: sha256:8686ba556bab3b5149f39e3f4ec6c82f0cc79d3181caa2773900491a6baf3713
Deleted: sha256:eda7136a91b7b4ba57aee64509b42bda59e630afcb2b63482d1b3341bf6e2bbb
Untagged: centos:latest
Untagged: centos@sha256:eba772bac22c86d7d6e72421b4700c3f894ab6e35475a34014ff8de74c10872e
Deleted: sha256:196e0ce0c9fbb31da595b893dd39bc9fd4aa78a474bbdc21459a3ebe855b7768
Deleted: sha256:cf516324493c00941ac20020801553e87ed24c564fb3f269409ad138945948d4
Untagged: ubuntu:trusty
Untagged: ubuntu@sha256:6e3e3f3c5c36a91ba17ea002f63e5607ed6a8c8e5fbbddb31ad3e15638b51ebc
Deleted: sha256:dea1945146b96542e6e20642830c78df702d524a113605a906397db1db022703
Deleted: sha256:6401e3024b4d4ef4c981cde2e830858eb790ee84284e1401cf569a6db8df51d9
Deleted: sha256:f12ee38eb7aa0ffdd43c657b433d91ac4c2930887c02eb638fd1518f374bc738
Deleted: sha256:9ac64e2751425199591402799079940629829c7c2fc0e083fb714e5dd94d70a9
Deleted: sha256:12a6279e654d2f23c2fa086bf2dcd82e1a2c82b01028379bbf2cde061d9235e6
Deleted: sha256:c47d9b229ca4eaf5d3b85b6fa7f794d00910a42634dd0fd5107a9a937b13b20f
```

### 우분투 (ubuntu 16.04) 컨테이너 실행하기

- 해당 우분투 이미지가 없으면 알아서 설치하고나서 우분투를 실행함.
- `docker run ubuntu:16.04`
- 이때 우분투 이미지를 새로 설치하고나서 바로 종료됨.
- 설치가 완료되고, 옵션과 함께 다시 실행해 봅시다.
- `docker run --rm -it ubuntu:16.04 /bin/bash`
- 새로운 우분투 컨테이너로 진입한 상태를 알아챌 수 있을 것.
- `cat /etc/issue` 명령어를 통해서 우분투 버전을 확인해봅시다.
- –rm 옵션 때문에 exit 하는 순간 모든 데이터 삭제됨.

### Redis 컨테이너 실행하기

- detached mode (백그라운드모드)로 : -d 옵션
- 컨테이너 포트를 호스트의 포트로 연결 : -p 옵션
- `docker run -d -p 1234:6379 redis`
- 없으니까 바로 다운로드 받고 실행됨
- -p 옵션을 이용: 호스트의 1234포트를 컨테이너의 6379포트로 연결, localhost의 1234포트로 접속하면 하면 redis를 사용 가능

```
~$ docker run -d -p 1234:6379 redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
065132d9f705: Pull complete 
be9835c27852: Pull complete 
f4a0d1212c38: Pull complete 
43be9e9f0fb9: Pull complete 
a1bca8e532ec: Pull complete 
382eae952932: Pull complete 
Digest: sha256:ebb396dc3ac00e8eb4a64c1c022ef41ef16801f31ff98b16916a77fdc7252e67
Status: Downloaded newer image for redis:latest
6c9e33af12720e6d5ad70506e709d7bfddc621ae9887270dd6da4be93fdea964
```

- 텔넷으로 연결해보기

```
~$ telnet localhost 1234
Trying ::1...
Connected to localhost.
Escape character is '^]'.
#아래와같이 명령어를 입력해보자. 
set mykey hello
+OK
get mykey
$5
hello
```