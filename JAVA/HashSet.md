HashSet 클래스에 대해서 알아보겠습니다

 

> **목차**
>
> **[HashSet이란?](#text1)**
> **[중복을 걸러내는 과정](#text2)**
> **[HashSet 변수 선언](#text3)**
> **[HashSet 값 추가](#text4)**
> **[HashSet 값 삭제](#text5)**
> **[HashSet의 크기 구하기](#text6)**
> **[HashSet 데이터 출력하기](#text7)**
> **[HashSet 검색하기](#text8)**

------



## **HashSet이란?**

일단 자바의 HashSet 정의한 것을 참고해서 설명해보겠습니다

Set 인터페이스에서 지원하는 구현 클래스입니다.

순서대로 입력되지 않고, 일정하게 유지되지 않는게 특징입니다.

HashSet은 null 요소도 허용합니다

이 클래스의 가장 큰 특징은 중복을 허용하지 않는다는 것 입니다

 

------



## **중복을 걸러내는 과정**

HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode()메소드를 호출해서 해시 코드를 얻어낸 다음 저장되어 있는 객체들의 해시 코드와 비교한 뒤 같은 해시 코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않습니다. 

문자열을 HashSet에 저장할 경우, 같은 문자열을 갖는 String객체는 동일한 객체로 간주되고 다른 문자열을 갖는 String객체는 다른 객체로 간주되는데, 그 이유는 String클래스가 hashCode()와 equals() 메소드를 재정의해서 같은 문자열일 경우 hashCode()의 리턴 값을 같게, equals()의 리턴 값은 true가 나오도록 했기 때문입니다.

 

 

------



## **HashSet 변수 선언**

HashSet의 변수를 선언하는 방법입니다

HashSet<데이터타입> 변수명 = new HashSet<데이터타입>(); 으로 선언해줍니다

HashSet<Integer> : Integer형의 HashMap 데이터가 들어갑니다

HashSet<String> : String형의 HashMap 데이터가 들어갑니다

```
HashSet<Integer> set = new HashSet<Integer>();		
HashSet<String> set2 = new HashSet<String>();
```

 

------



## **HashSet 값 추가**

HashSet의 값을 추가하는 방법입니다

HashSet의 add(value) 메소드를 사용하여 값을 추가합니다

추가되는 값은 HashSet<데이터타입>의 맞는 데이터만 추가해줍니다!

```
public class HashSetTest {
	public static void main(String[] args)  {	
		// Integer
		HashSet<Integer> set = new HashSet<Integer>();	
		
		set.add(1);
		set.add(2);
		set.add(3);
		set.add(1);
				
		// String
		HashSet<String> set2 = new HashSet<String>();

		set2.add("a");
		set2.add("b");
		set2.add("c");
		set2.add("a");
	}
}
```

 

------



## **HashSet 값 삭제**

HashSet의 값을 삭제하는 방법입니다

HashSet의 remove(value) 메소드를 사용하면 원하는 value 값만 삭제가 됩니다

전부 삭제하고 싶은 경우 HashSet의 clear() 메소드를 사용해줍니다

```
public class HashSetTest {
	public static void main(String[] args)  {	
		// Integer
		HashSet<Integer> set = new HashSet<Integer>();	
		set.remove(1);
		set.clear();
				
		// String
		HashSet<String> set2 = new HashSet<String>();
		set2.remove("a");
		set2.clear();
	}
}
```

 

------



## **HashSet의 크기 구하기**

HashSet의 size() 메소드를 사용해 현재 HashSet의 크기를 구할 수 있습니다

아래와 같이 중복값이 들어오면 자동으로 제거됩니다

Size() 메소드 사용시 둘 다 결과가 3으로 출력이 됩니다

```
public class HashSetTest {
	public static void main(String[] args)  {	
		// Integer
		HashSet<Integer> set = new HashSet<Integer>();	
		
		set.add(1);
		set.add(2);
		set.add(3);
		set.add(1);
		System.out.println("set의 크기 : " + set.size());
				
		// String
		HashSet<String> set2 = new HashSet<String>();

		set2.add("a");
		set2.add("b");
		set2.add("c");
		set2.add("a");
		System.out.println("set2의 크기 : " + set2.size());
	}
}
```

결과 화면



![img](https://blog.kakaocdn.net/dn/bPOcPZ/btq36Sk2yBM/ErbWDxXPFuKciiFx5JtdKK/img.png)



 

------



## **HashSet 데이터 출력하기**

HashSet 데이터를 단순히 println으로 출력을 하는 경우 [1, 2, 3], [a, b, c] 형태로 출력을 하게 됩니다

하나의 객체를 가져오고 싶을 경우 Iterator를 사용해서 가져올 수 있습니다

```
import java.util.HashSet;
import java.util.Iterator;

public class HashSetTest {
	public static void main(String[] args)  {	
		// Integer
		HashSet<Integer> set = new HashSet<Integer>();	
		
		set.add(1);
		set.add(2);
		set.add(3);
		set.add(1);
		System.out.println("set의 값 : " + set);
				
		// String
		HashSet<String> set2 = new HashSet<String>();

		set2.add("a");
		set2.add("b");
		set2.add("c");
		set2.add("a");
		System.out.println("set2의 값 : " + set2);
		
		// Integer 출력
		Iterator iter = set.iterator();
		while(iter.hasNext()) {
			System.out.print(iter.next() + " ");
		}
		
		System.out.println("");

		// String 출력
		Iterator iter2 = set2.iterator();
		while(iter2.hasNext()) {
			System.out.print(iter2.next() + " ");
		}
	}
}
```

결과 화면



![img](https://blog.kakaocdn.net/dn/PU8gg/btq38eae6ry/oR1sB7hWQ49c8TNjbw4yu1/img.png)



 

------



## **HashSet 검색하기**

Hashet 내부의 원하는 값을 검색하는 경우 contains(value) 메소드를 사용합니다

값이 존재한다면 true, 값이 없다면 false를 return합니다

```
public class HashSetTest {
	public static void main(String[] args)  {	
		// Integer
		HashSet<Integer> set = new HashSet<Integer>();	
		
		set.add(1);
		set.add(2);
		set.add(3);
		set.add(1);
		
		System.out.println("1은 있는가? : " + set.contains(1));
				
		// String
		HashSet<String> set2 = new HashSet<String>();

		set2.add("a");
		set2.add("b");
		set2.add("c");
		set2.add("a");
		
		System.out.println("a는 있는가? : " + set2.contains("a"));
	}
}
```

결과 화면



![img](https://blog.kakaocdn.net/dn/b0svch/btq38kOZJpw/a3YAYrVon4GErNGC2nmj21/img.png)



 

여기까지 HashSet 클래스에 대해서 알아봤습니다