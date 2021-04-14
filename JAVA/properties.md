properties란 무엇이고, 어떻게 사용하는지 방법에 대해서 간략하게 알아보겠습니다.

 

------

## **properties란?**

 Key=Value형식으로 파라미터 정보들을 저장하기 위한 파일 확장자를 의미합니다. 주로 응용 프로그램에 대한 환경설정정보, DB와 연결하기 위한 DB환경설정정보 등을 저장할 때 properties파일을 만들어 그 곳에 저장해놓습니다. 

 주석처리를 하고 싶은 경우 맨 앞에 "#", "!"을 붙여 주석처리를 합니다.

 properties와 비슷한 녀석으로는 xml, yml이 있습니다.

 

------

## **properties사용예제**

 자바 프로젝트에 properties파일을 추가합니다.

간단하게 테스트용으로 HelloWorld.properties파일을 추가했습니다.

#### **HelloWorld.properties**

```
HELLO_WORLD=Hello World!!
ONE_PLUS_ONE=1+1=2
```

위와 같이 Key=Value 2개를 추가했습니다.

 

#### **PropertiesTest.java**

```
import java.util.Locale;
import java.util.ResourceBundle;

public class PropertiesTest {

	public static void main(String[] args) {
		ResourceBundle rb		= null;
		rb = ResourceBundle.getBundle("HelloWorld", Locale.KOREA);
		
		String helloWorld = rb.getString("HELLO_WORLD");
		String onePone = rb.getString("ONE_PLUS_ONE");
		
		System.out.println(helloWorld);
		System.out.println(onePone);
	}
}
```

java util의 ResourceBundle을 사용하여 properties의 값을 호출해줍니다. 위의 코드를 참고바랍니다.

아래는 위의 예제 코드를 실행했을 때 결과화면입니다.



![img](https://blog.kakaocdn.net/dn/c62pzZ/btrveLTvHz7/zFBkKic1tFmL6WWWRN86eK/img.png)



