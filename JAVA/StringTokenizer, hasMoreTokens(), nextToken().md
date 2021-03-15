JAVA의 기본 클래스인 StringTokenizer와 StringTokenizer 클래스 밑의 메소드 hasMoreTokens(), nextToken()에 대해서 알아보겠습니다

 

> **목차**
>
> **[StringTokenizer](#text1)**
> **[hasMoreToken()](#text2)**
> **[nextToken()](#text3)**
> **[StringTokenizer 구분자 지정하기](#text4)**
> **[new StringTokenizer(문자열, 구분자, true/false)](#text5)**

 

------



## **StringTokenizer**

StringTokenizer의 역할은 String에서 구분자를 통해서 토큰형태로 나눌 때 사용하는 클래스입니다

기본적으로 구분자 집합에는 "\t\n\r\f"를 사용합니다

즉, 공백문자, 탭문자, 새줄문자 등의 문자를 말합니다

하지만 구분 기호 문자 자체는 토큰으로 처리되지 않습니다

예를 들어서 설명하겠습니다

StringTokenizer변수 st에는 a, b, c, d가 나눠서 들어가있는 상태입니다

```
import java.util.StringTokenizer;

public class ST_Test {
	public static void main(String[] args)  {	
		String str = "a b c d";
		
		StringTokenizer st = new StringTokenizer(str);	
	}
}
```

 

------



## **hasMoreToken()**

StringTokenizer에 사용할 수 있는 토큰이 더 있는지 확인합니다

이 메서드가 ture를 반환하는 경우는 토큰이 존재하고, false를 반환하는 경우 토큰이 없다는 것 입니다

 

------



## **nextToken()**

StringTokenizer에서 다음 토큰을 불러오는 메서드입니다

StringTokenizer와 hasMoreToken, nextToken을 사용하여 프로그램을 구현했습니다

이 경우 a, b, c, d를 하나의 토큰으로 쪼개어 출력해줍니다

```
import java.util.StringTokenizer;

public class ST_Test {
	public static void main(String[] args)  {	
		String str = "a b c d";
		
		StringTokenizer st = new StringTokenizer(str);	
		
		while(st.hasMoreTokens())
		{
			System.out.print(" " + st.nextToken());
		}
	}
}
```

결과화면



![img](https://blog.kakaocdn.net/dn/mwjwK/btq4bjIbAft/PparUvVJjOOVVTQlyJQn90/img.png)



 

------



## **StringTokenizer 구분자 지정하기**

StringTokenizer에 구분자를 추가하는 방법이다

원래라면 탭 문자, 공백 문자, 새줄 문자 이 3개가 기본문자열이지만 a와 c를 구분자로 추가하였다

결과를 확인하면 a와 c가 제외되어있다

```
import java.util.StringTokenizer;

public class ST_Test {
	public static void main(String[] args)  {	
		String str = "a b c d e f g";
		
		StringTokenizer st = new StringTokenizer(str, "ac ");	
		
		while(st.hasMoreTokens())
		{
			System.out.print(" " + st.nextToken());
		}
	}
}
```

결과 화면



![img](https://blog.kakaocdn.net/dn/bxhTca/btq39VnoVqq/GeL1iu2fv0NuacIdCmPXAK/img.png)



 

------



## **new StringTokenizer(문자열, 구분자, true/false)**

StringTokenizer의 다른 사용법이다

예시를 통해 설명하겠다

\1. true로 넘긴 경우 구분자까지 모두 표시된다

```
import java.util.StringTokenizer;

public class ST_Test {
	public static void main(String[] args)  {	
		String str = "a b c d e f g";
		
		StringTokenizer st = new StringTokenizer(str, "ac ", true);	
		
		while(st.hasMoreTokens())
		{
			System.out.print(" " + st.nextToken());
		}
	}
}
```

결과화면



![img](https://blog.kakaocdn.net/dn/tea6a/btq33brNPCA/qsn1X3c1ZC0C3l6ThzkfR1/img.png)



 

\2. false로 넘긴 경우 구분자를 표시하지 않는다

```
import java.util.StringTokenizer;

public class ST_Test {
	public static void main(String[] args)  {	
		String str = "a b c d e f g";
		
		StringTokenizer st = new StringTokenizer(str, "ac ", false);	
		
		while(st.hasMoreTokens())
		{
			System.out.print(" " + st.nextToken());
		}
	}
}
```

결과화면



![img](https://blog.kakaocdn.net/dn/cmhJcz/btq36rgBncc/BVRNTkKgvbhFoYmmoQyrrk/img.png)



여기까지 **StringTokenizer, hasMoreTokens(), nextToken()**에 대해서 알아봤습니다