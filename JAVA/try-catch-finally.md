try - catch - finally 문법에 대해서 알아보겠습니다.

제 개인적으로 공부를 위해 정리하는 용도라 틀린 부분이 있을 수 있습니다. 그 부분에 대해서는 댓글 남겨주시면 감사하겠습니다.

 

------

## **try - catch문을 사용하는 이유**

 기본적으로 프로그램에서 에러가 나는 경우 프로그램은 중단이 됩니다. 아무리 잘 만든 프로그램이어도 오류가 단 한 번도 없이 돌아가기란 힘듭니다. 프로그램에서 오류가 난 경우 그에 대한 예외처리를 하기 위해 **try - catch문**을 사용합니다. 

 

------

## **try - catch 사용방법**

```
try {
	/* 
      프로그램에서 사용하는 일반적인 코드를 입력합니다.
      코드를 실행 중 에러가 나면 그 자리에서 중단되고 catch문으로 이동합니다.
      오류가 없다면 try 안의 구문을 모두 실행합니다.
    */
} catch(Exception e) {
	/*
      try에서 오류가 나면 catch안의 내용을 실행합니다.
      try에 오류가 없다면 catch는 실행하지 않습니다.
    */
}
```

위와 같은 방법으로 try - catch구문을 사용합니다.

 

아래는 예제입니다.

```
public class ExceptionTest {
	public static void main(String[] args) {
		try {
			System.out.println("Exception STEP 1 =======");			
			throw new Exception();	
			
		} catch (Exception e) {
			System.out.println("Exception STEP 2 =======");
		}
	}
}
```

위의 프로그램을 실행하면 STEP 1, STEP 2가 출력되는 것을 볼 수 있습니다. 위에는 설명을 안 했지만 **throw new Exception() 문법을 사용하여 Exception을 강제**로 낼수도 있습니다.

 

**결과화면**



![img](https://blog.kakaocdn.net/dn/4ItkK/btrvpaSg2Y7/JhbNZw1rXEIgu2mdltDGv1/img.png)



 

------

## **try - catch - finally**

 이번에는 **finally문법**에 대해서 알아보겠습니다. try블록에서 일어난 일과 관계없이 finally안의 구문을 실행합니다. 

try - catch - finally를 모두 사용할 필요없이, try - finally로 사용할 수 있습니다. 하지만 try하나만 사용은 불가능합니다. **즉 try문법에서 catch와 finally는 둘 중 하나는 생략이 가능하지만, 둘 다 생략은 불가능합니다.**

```
try {
} catch(Exception e) {
} finally {
}
```

위의 처럼 사용이 가능합니다. 실제로 사용한 예제로 알아보겠습니다.

 

**사용예제**

try - catch - finally에서 아래와 같이 **try - finally**만 사용이 가능합니다. 해당 프로그램을 실행하면 STEP 1, STEP 2 결과가 출력됩니다.

```
public class ExceptionTest {
	public static void main(String[] args) {
		try {
			System.out.println("Exception STEP 1 =======");	
		} finally {
			System.out.println("Exception STEP 2 =======");				
		}
	}
}
```

**결과화면**



![img](https://blog.kakaocdn.net/dn/wKcOE/btrvkFTAOz6/f5GLnG3gZN60kt4feRzxW0/img.png)



 

try - catch - finally를 실행하면 try안의 STEP 1과 finally안의 STEP 3이 결과로 출력이 됩니다.

```
public class ExceptionTest {
	public static void main(String[] args) {
		try {
			System.out.println("Exception STEP 1 =======");	
		} catch(Exception e) {
			System.out.println("Exception STEP 2 =======");				
		} finally {
			System.out.println("Exception STEP 3 =======");				
		}
	}
}
```

**결과화면**



![img](https://blog.kakaocdn.net/dn/clnbmq/btrvgmNWIJj/65erK9hsTfxHBI9MYWeLjK/img.png)



 

try - catch - finally문법에서 try에 강제로 에러를 발생시켜 보겠습니다. STEP 1, 2, 3이 모두 결과로 출력된 것을 확인할 수 있습니다.

```
public class ExceptionTest {
	public static void main(String[] args) {
		try {
			System.out.println("Exception STEP 1 =======");	
			throw new Exception();
		} catch(Exception e) {
			System.out.println("Exception STEP 2 =======");				
		} finally {
			System.out.println("Exception STEP 3 =======");				
		}
	}
}
```

**결과화면**



![img](https://blog.kakaocdn.net/dn/bPykoC/btrvngMtfhb/8ERiIgfOYeEOC2SZujjgN0/img.png)

