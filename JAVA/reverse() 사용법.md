문자열 반전을 구현하면서 reverse()를 사용하여 구현을 했었는데

이번 포스팅에서는 reverse() 정의 및 사용 방법에 대해서 알아보겠습니다

 

> **목차**
>
> **[reverse()란?](#text1)**
> **[StringBuilder 사용](#text2)**
> **[StringBuffer 사용](#text3)**
> **[Collections 사용](#text4)**

 

------



## **reverse()란?**

reverse는 직역하면 뒤바꾸다, 반전시키다가 됩니다

문자열 뒤집는 메서드입니다

 

하지만 String클래스에서는 reverse()를 제공하지 않아 변환해서 reverse()를 사용해야합니다

StringBuilder, StringBuffer, Collection클래스에서 reverse() 메서드를 제공합니다

String 문자열을 반전시키기 위해서는 위의 클래스로 변환해서 사용해야합니다

 

------



## **StringBuilder 사용**

StringBuilder클래스의 메서드 reverse()를 사용하여 문자열을 반전시킵니다

**사용 예제**

```
public class Reverse {
	public static void main(String[] args)  {	
        String input = "Hello World!";

        // StringBuilder 선언
        StringBuilder input1 = new StringBuilder();
 
        // StringBuilder에 입력
        input1.append(input);
 
        // reverse 메서드 사용
        input1.reverse();
 
        // 결과 출력
        System.out.println(input1);
	}
}
```

 

**결과 화면**



![img](https://blog.kakaocdn.net/dn/kACa3/btq49aYrYuj/QoGKed1xsGxu9nADiLJhDk/img.png)



 

------



## **StringBuffer 사용**

StringBuilder클래스의 메서드 reverse()를 사용하여 문자열을 반전시킵니다

**사용 예제**

```
public class Reverse {
	public static void main(String[] args)  {	
        String str = "Hello World!";

        // StringBuilder 선언 및 입력
        StringBuffer sbr = new StringBuffer(str);

        // reverse 메서드 사용
        sbr.reverse();

        // 결과 출력
        System.out.println(sbr);
	}
}
```

 

**결과 화면**



![img](https://blog.kakaocdn.net/dn/bAidCa/btq42zZv2nQ/KlwtRKLuDUYBKnvBBPpzZk/img.png)



 

------



## **Collections 사용**

객체를 담는 클래스인 Collections에서도 reverse()를 사용이 가능합니다

String을 Collections 형태로 convert하려면 약간 복잡한 과정을 거쳐야합니다

\1. String을 Char[]배열로 convert 합니다

\2. Char[]배열에서 ArrayList<>형태로 Convert 합니다

\3. Collections의 reverse를 사용하여 ArrayList를 반전시킵니다

\4. ListIterator 객체에 String 목록을 넣고 결과를 출력시켜줍니다

```
import java.util.*;

public class Reverse {
	public static void main(String[] args)  {	
        String input = "Hello World!";
        
        // char[]배열에 String값 convert
        char[] hello = input.toCharArray();
        
        // ArrayList 선언
        List<Character> trial1 = new ArrayList<>();
 
        // char[]배열의 값 ArrayList에 입력
        for (char c : hello)
            trial1.add(c);
 
        // Collections를 사용 ArrayList 값 반전
        Collections.reverse(trial1);
        
        // 값을 출력하기위한 ListIterator 선언 및 출력
        ListIterator li = trial1.listIterator();
        while (li.hasNext())
            System.out.print(li.next());
	}
}
```

 

이상 자바의 reverse()의 여러가지 사용법에 대해서 알아봤습니다