LinkedHashSet의 개념과 사용법에 대해서 알아보겠습니다!

 

> **목차**
>
> **[LinkedHashSet이란?](#text1)**
> **[LinkedHashSet 선언하기](#text2)**
> **[LinkedHashSet 값 추가하기](#text3)**
> **[LinkedHashSet 값 삭제하기](#text4)**
> **[LinkedHashSet 크기 구하기](#text5)**
> **[LinkedHashSet 값 출력하기](#text6)**

------



## **LinkedHashSet이란?**

HashSet과 동일한 구조를 가지지만 HashSet은 순서를 관리하지 않아 값을 출력할 때마다 다른 순서대로 출력이 됩니다

하지만 LinkedHashSet은 삽입된 순서대로 반복합니다

HashSet과 동일한 특징들이 있는데 마찬가지로 중복 값을 허용하지 않습니다

 

------



## **LinkedHashSet 선언하기**

```
import java.util.Arrays;
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet hs = new LinkedHashSet(); // 타입 설정x Object 입력
		LinkedHashSet<LinkedHashSetDemo> demo = new LinkedHashSet<LinkedHashSetDemo>(); // 클래스로 타입 설정
		LinkedHashSet<Integer> i = new LinkedHashSet<Integer>(); // Integer 타입 선언
		LinkedHashSet<Integer> i2 = new LinkedHashSet(); // 뒷부분 타입 선언 생략 가능
		LinkedHashSet<Integer> i3 = new LinkedHashSet<Integer>(10); // 크기 10으로 선언
		LinkedHashSet<Integer> i4 = new LinkedHashSet<Integer>(Arrays.asList(1, 2, 3, 4)); // 선언과 동시에 초기 값 설정
		
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // String 타입 선언
		LinkedHashSet<Character> ch = new LinkedHashSet<Character>(); // Char 타입 선언		
	}
}
```

LinkedHashSet의 선언하는 방법입니다

LinkedHashSet 변수명 = new LinkedHashSet(); 으로 기본적으로 선언이 가능합니다

LinkedHashSet<타입> 변수명 = new LinkedHashSet<타입>(); 으로 LinkedHashSet의 타입을 설정 가능합니다



타입은 클래스, Integer, String, Character 등 다양한 방식으로 선언이 가능합니다

LinkedHashSet<타입>(크기); 를 하면 크기를 설정가능합니다

LinkedHashSet<타입>(Arrays.asList(값)); 초기값을 설정하면서 선언이 가능합니다

이렇게 다양한 방법으로 변수를 선언할 수 있습니다

 

------



## **LinkedHashSet 값 추가하기**

```
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // LinkedHashSet 선언
		
		// 값 추가
		str.add("Hello1");
		str.add("World2");
		str.add("Hello3");
		str.add("World4");
		str.add("World2");
				
		System.out.print(str); // 결과 출력
	}
}
```

LinkedHashSet의 값을 추가하는 방법입니다

add(Object) 메서드를 사용하여 값을 추가합니다

예제를 보면 "World2"를 추가하지만 중복값이라 결과를 조회할 때는 제외하고 출력된 것을 볼 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cvC0ik/btq5AQer5DY/98Xspz894v9QL8h5vc2Rq0/img.png)



 

------



## **LinkedHashSet 값 삭제하기**

```
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // LinkedHashSet 선언
		
		// 값 추가
		str.add("Hello1");
		str.add("World2");
		str.add("Hello3");
		str.add("World4");
		
		System.out.println(str); // 결과 출력		
		
		str.remove("World2"); // World2 값 삭제
		System.out.println(str); // 결과 출력	
		
		str.clear(); // 모든 값 삭제
		System.out.println(str); // 결과 출력	
	}
}
```

LinkedHashSet에서 값을 삭제하는 방법입니다

LinkedHashSet에서는 Linked로 서로 연결하여 값을 관리하기때문에 순서만 관리할 뿐 앞/뒤 데이터만 삭제가 불가능합니다

remove(Obejct) 메서드를 사용하여 원하는 값을 제거가 가능합니다

clear() 메서드를 사용하면 모든 값을 삭제합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/szobV/btq5xtShou3/yW11dUKXqJdGkg8srQGkL1/img.png)



 

------



 

 

## **LinkedHashSet 크기 구하기**

```
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // LinkedHashSet 선언
		
		// 값 추가
		str.add("Hello1");
		str.add("World2");
		str.add("Hello3");
		str.add("World4");
		
		System.out.println(str); // 결과 출력	
		System.out.println("LinkedHashSet의 크기는? " + str.size()); // 결과 출력	
	}
}
```

LinkedHashSet의 크기를 구하는 방법은 size() 메서드를 사용하면 됩니다

LinkedHashSet 안에 있는 값의 갯수를 출력해줍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bTJxAn/btq5APmiNgz/19g9Om5g8421CALniLpKk0/img.png)



 

------



## **LinkedHashSet 값 출력하기**

```
import java.util.Iterator;
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
	public static void main(String[] args)  {
		LinkedHashSet<String> str = new LinkedHashSet<String>(); // LinkedHashSet 선언
		
		// 값 추가
		str.add("Hello1");
		str.add("World2");
		str.add("Hello3");
		str.add("World4");
		
		/* Iterator를 사용 HashSet 배열 출력 */
		Iterator iter = str.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

LinkedHashSet의 값을 출력하는 방법입니다

하나의 값만 출력하는 메서드는 따로 제공하지 않아 Iterator를 사용하여 값을 출력해줘야합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/b2m0g6/btq5FhigW22/azsK20XEdCpsveWoxd4lkk/img.png)

