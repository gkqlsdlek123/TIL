# 이미지 형태전환 - Erosion and Dilation



## 이미지 형태 전환 (Morphological Transformations)

- 이미지 형태 전환은 기본적으로 Binary Image 상에서 이루어진다.
- 두 개의 input 이 필요하다. (원본 이미지, 커널)
- 커널은 Image Transformation 을 결정하는 구조화된 요소
- Erosion 은 이미지를 침식시키는 것
- Dilation 은 이미지를 팽창시키는 것
- 커널의 크기가 크거나, 반복횟수가 많아지면 과하게 적용되어 경계가 없어질 수도 있다.



## Erosion

- Erosion 은 이미지를 침식시키는 것
- Foreground 가 되는 이미지의 경계부분을 침식시켜서 Background 이미지로 전환한다.
- Foreground 이미지가 가늘게 된다.
- 흐릿한 경계부분은 배경으로 만들어버린다고 생각하면 좋을 듯
- 이미지는 Matrix 이다. 작은 (n x n) 커널 창으로 이미지 전체를 훝으면서 커널창에 들어온 matrix 값들을 변경한다.
- 메서드는 아래와 같다.
- `erosion = cv2.erode(img, kernel, iterations=1)`
  - img: erosion을 수행할 원본 이미지
  - kernel: erosion을 위한 커널
  - iterations: Erosion 반복 횟수



## Dilation

- Dilation 은 이미지를 팽창시키는 것
- Erosion과 반대로 동작한다.
- 메서드는 아래와 같다.
- `dilation = cv2.dilate(img, kernel, iterations=1)`
  - img: erosion을 수행할 원본 이미지
  - kernel: erosion을 위한 커널
  - iterations: Erosion 반복 횟수



### Erosion과 Dilation 을 적용한 코드 실습

- 아래와 같이 코드를 작성한다.

```
def erosion_dilation():
    filename = 'images/ad_text2.jpg'
    image = cv2.imread(filename, cv2.IMREAD_GRAYSCALE)  #// 회색조로 이미지 객체를 생서한다.

    #// make kernel matrix for dilation and erosion (Use Numpy)
    kernel_size_row = 3
    kernel_size_col = 3
    kernel = np.ones((3, 3), np.uint8)

    erosion_image = cv2.erode(image, kernel, iterations=1)  #// make erosion image
    dilation_image = cv2.dilate(image, kernel, iterations=1)  #// make dilation image

    # open image window
    cv2.namedWindow('erosion_image',
                    cv2.WINDOW_NORMAL)  #// 윈도우 창의 성격 지정 인자 : (윈도우타이틀, 윈도우 사이즈 플래그) , 사용자가 크기 조절할 수 있는 윈도우 생성
    cv2.imshow('erosion_image', erosion_image)  #// ADAPTIVE_THRESH_GAUSSIAN_C 적용한 이미지 열기

    # open image window
    cv2.namedWindow('dilation_image',
                    cv2.WINDOW_NORMAL)  #// 윈도우 창의 성격z 지정 인자 : (윈도우타이틀, 윈도우 사이즈 플래그) , 사용자가 크기 조절할 수 있는 윈도우 생성
    cv2.imshow('dilation_image', dilation_image)  #// ADAPTIVE_THRESH_MEAN_C 적용한 이미지 열기

    # final process
    cv2.waitKey(0)  #// 키보드 입력될 때 까지 계속 기다림
    cv2.destroyAllWindows()  #// 이미지 윈도우 닫기
    
```

- 위 코드의 수행결과는 아래와 같다. 원본 -> Erosion 결과 -> Dilation 결과 순서이다.

![erosion and dilation](http://nicewoong.github.io/assets/erosion-and-dilation.png)

- Erosion 은 한 뭉치(?)가 더 가늘어지고, Dilation 은 한 뭉치가 좀더 굵어진다.



## 커널 매트릭스 (Kernel Matrix) 생성

![erosion-dilation-kernel](http://nicewoong.github.io/assets/erosion-dilation-kernel.png)

- image 는 사실 matrix data 입니다.
- kernel 이라는 작은 matrix 가 image data 전체를 훑으면서 효과를 적용합니다.
- kernel 사이즈를 조절하면서 이미지에 적용되는 효과를 조절할 수 있습니다.
- 위 예제에서 Numpy 를 이용하여 kernel matrix 를 생성했죠. 그런데 다른 방법이 있습니다.
- `M1 = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))` 메서드를 활용해서 더 손쉽게 Kernel Matrix 를 생성가능합니다.

```
// make kernel matrix for dilation and erosion (Use Numpy)
    kernel_size_row = 3
    kernel_size_col = 3
    kernel = np.ones((kernel_size_row, kernel_size_col), np.uint8)

// make kernel matrix for dilation and erosion (Use cv2 method)
    kernel_size_row = 3
    kernel_size_col = 3
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (kernel_size_row , kernel_size_col))
```



### 참고

- [readthedocs](http://opencv-python.readthedocs.io/en/latest/doc/12.imageMorphological/imageMorphological.html?highlight=erosion)
- [블로그포스트](http://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220505815055&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postList)