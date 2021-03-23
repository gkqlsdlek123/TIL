## Docker 컨테이너(Container) 커밋(Commit)하기

- 현재까지 작업해 놓은 ubuntu 컨테이너를 그대로 저장하고 싶다.

- 실행중인 컨테이너를 커밋해보자.

- `docker commit CONTAINER IMAGE_NAME` 명령 이용.

  ```
    $ docker commit ubuntu-cpp-driver ubuntu-cpp-driver
    sha256:85b8eb5a23e6c850c4f4d298119275ec5a85bc43c78414372aee2859e9ad9e54
  ```

- container 이름이 ubuntu-cpp-driver 였고, 이를 이미지로 저장할 때 이미지 이름도 동일하게 했다.

- `docker images`로 확인해보자.

  ```
    $ docker images
    REPOSITORY                  TAG                 IMAGE ID            CREATED              SIZE
    ubuntu-cpp-driver           latest              85b8eb5a23e6        About a minute ago   582MB
  ```

### 2. Docker 이미지(Image) Push하기

- Docker cloud 에 로그인 하기

  ```
    $ docker login
  ```

- docker user id 변수 지정해놓기

  ```
    $ export DOCKER_ID_USER="nicewoong"
  ```

- Docker Image 에 태그 달기

  ```
    $ docker tag ubuntu-cpp-driver $DOCKER_ID_USER/ubuntu-cpp-driver
  ```

- Tag 가 적용되어 있는 Image 를 Docker Cloud 에 Push

  ```
    $ docker push $DOCKER_ID_USER/ubuntu-cpp-driver
    The push refers to a repository [docker.io/nicewoong/ubuntu-cpp-driver]
    441a4ecb7164: Pushing [==================================================>]  472.2MB
    7f7a065d245a: Mounted from nicewoong/ubuntu-bluecoat 
    f96e6b25195f: Mounted from nicewoong/ubuntu-bluecoat 
    c56153825175: Mounted from nicewoong/ubuntu-bluecoat 
    ae620432889d: Mounted from nicewoong/ubuntu-bluecoat 
    a2022691bf95: Mounted from nicewoong/ubuntu-bluecoat 
  ```

