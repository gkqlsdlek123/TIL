### OpenCV 란?

- Computer Vision library 의 약자로 이미지 검수를 기계적으로 처리하도록 도와주는 이미지 처리 라이브러리
- Image Processing : Circle Detection, 이미지 선명하게 만들기, 필터를 이용해서 edge detection 등에 응용
- Robot/ Machine / Video / Vision : 얼굴인식, 무인자동차가 장애물 인식, 사람 객체 인식, 이미지 매칭 등에 응용
- AI : 딥러닝에 사용, 이미지가 어떤 내용인지 이해, 이미지의 얼굴 감정 인지 등에 응용가능
- 3D Geometry (기하학) : 이미지에서 3차원 정보를 인식할 수 있다. 여러 측면의 이미지를 조합해서 3D 도면 구성 가능 이와 같이 다양한 분야에서 응용될 수 있는 이미지 처리 라이브러리!!
- 영상 처리와 컴퓨터 비전 관련 오픈소스 라이브러리로 250개가 넘는 알고리즘으로 구성됨
- C, C++, Python 등 다양한 언어의 인터페이스를 갖춤
- 윈도우즈, 리눅스 안드로이드, 맥 os 다양한 운영체제를 지원



### OpenCV-Python

- OpenCV의 Python API
- 파이썬은 스크립트 언어이기 때문에 C, C++ 과 같은 컴파일 언어에 비해 속도가 느림
- 성능적인 이슈가 있는 부분은 C/C++로 구현한 후 이를 파이썬 Wrapper 를 만들면 해결 가능



### Python3 설치 및 Pycharm 개발 환경 구성

- 참고하는 블로그 포스트가 Python3.x 와 OpenCV 3.2.x + contrib 버전을 적용하고 있다. 그래서 Python3 를 현재 윈도우 노트북에 설치한다.
- 그런데 Python3 를 설치하고 환경변수가 제대로 설정되었는지 까지 몇 번을 확인했지만 cmd 에서 python3 를 사용할 수가 없었다.
- python3 를 현재 윈도우 os 에 접속한 local user 하위 폴더에 설치(default 설정)했었다. local user 의 환경변수와 시스템 환경 변수 설정을 확인하고도 여전히 python3를 cmd 에서 사용할 수 없었다.
- 그래서 python3 를 우선 삭제하고 global 적용이 되도록 재설치 했다. Install Now를 누르는 대신 Customize installation 을 선택해서 global 관련 항목(all user 가 사용할 수 있도록 설치한다는 내용 )을 체크하고 설치를 했다.
- 설치 첫화면 하단 Add Python 3.xx to PATH 역시 체크했다. 환경변수 PATH에 python3를 등록하는 것이다. 그러면 어느 위치에 있든 상관 없이 python3를 사용할 수 있는 것

![img](http://nicewoong.github.io/assets/python_win_installer.png)

- 이렇게 all user 가 사용할 수 있도록 재설치를 하고 cmd 를 켜서 python –version 명령어로 확인을 하니 Python3 버전이 출력되었다. 윈도우에서는 바로 python3 가 아니라 python 명령에 3 버전이 등록되어 버리네.

```
>python --version  # check python version 
Python 3.6.4

>pip --version  #  check python version of pip 
pip 9.0.1 from c:\program files\python36\lib\site-packages (python 3.6) 
```

- Pycharm Project 를 실행시키고 Project interpreter 를 Python 3.6 으로 설정하니 Pycharm 이 알아서 Virtual Environment 를 생성한다. 이 과정이 왜 자동으로 일어나는지 잘 모르겠다. 그리고 Virtual Environment 가 여러 project 에서 python package 가 충돌나지 않도록 하기 위한 것이라는 건 알겠지만 그 자세한 개념을 잘 모르겠다. 공부가 필요한 부분이다.
- venv 과 pycharm 에서 생성되는 폴더 및 파일들은 제외하고 GitHub 에 올리기 위해서 .gitignore 파일을 생성한다.
- 그리고 일단 Git-hub에 프로젝트를 만들고 push 했다.
- 자 이제 본격적으로 튜토리얼을 따라해보자!



### 필요 라이브러리 설치하기

- Pycharm 하단의 Terminal 창에서 pip 명령어를 통해 필요한 라이브러리르 설치한다.
- 아 근데 터미널에서 바로 pip install 명령어를 이용해서 설치하면 되는데, 왜 다른 글에서는 파이썬 스크립트 폴더에 패키지를 먼저 다운받으라고 하는 걸까?

```
>pip install matplotlib
>pip install numpy
>pip install OpenCV-Python

# 아래는 OpenCV 설치 중 출력 결과
Collecting OpenCV-Python
  Downloading opencv_python-3.4.0.12-cp36-cp36m-win_amd64.whl (33.3MB)
    100% |████████████████████████████████| 33.4MB 19kB/s
Requirement already satisfied: numpy>=1.11.3 in c:\users\viva\pycharmprojects\opencv-practice\venv\lib\site-packages (from OpenCV-Python)
Installing collected packages: OpenCV-Python
Successfully installed OpenCV-Python-3.4.0.12
```

- OpenCV가 잘 설치되었는지 확인을 위해서 python console 에서 import cv2 를 수행해본다. 에러메시지가 출력되지 않는다면 성공!
- python code 상단에는 항상 encoding 설정을 추가하자.

```
# -*- coding: utf-8 -*-
```



### 이미지 열기

```
def showImage():
    FILENAME = 'images/bus_people.jpg'
    # 이미지 파일을 읽기 위한 객체를 리턴  인자(이미지 파일 경로, 읽기 방식)
    # cv2.IMREAD_COLOR : 투명한 부분 무시되는 컬러
    # cv2.IMREAD_GRAYSCALE : 흑백 이미지로 로드
    # cv2.IMREAD_UNCHANGED : 알파 채컬을 포함한 이미지 그대로 로드
    image = cv2.imread(FILENAME, cv2.IMREAD_GRAYSCALE)
    cv2.namedWindow('model', cv2.WINDOW_NORMAL)  #윈도우 창의 성격 지정 인자 : (윈도우타이틀, 윈도우 사이즈 플래그) , 사용자가 크기 조절할 수 있는 윈도우 생성
    cv2.imshow('model', image)  # 화면에 이미지 띄우기 인자;(윈도우타이틀, 이미지객체)
    cv2.waitKey(5000)  # 지정된 시간 동안 키보드 입력 대기 (ms) , 0이면 key 입력할 때 까지 계속 대기, 입력받은 key 값을 int 로 반환 (아스키코드)
    # 대기시간이 끝나면 아래 코드로 진행
    cv2.destroyAllWindows()  # 생성한 윈도우 제거
    
```



### 참고

- [OpenCV-Python 블로그 정리](http://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220498694383&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postView)
- [공식홈페이지(OpneCV : Computer vision library)](https://opencv.org/)