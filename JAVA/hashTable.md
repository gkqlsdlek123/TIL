HashMap에 대해서 알아보겠습니다

 

> **목차**
>
> **[HashMap이란?](#text1)**
> **[HashMap 선언하기](#text2)**
> **[HashMap 값 추가하기](#text3)**
> **[HashMap 값 삭제하기](#text4)**
> **[HashMap 크기 구하기](#text5)**
> **[HashMap 값 출력하기](#text6)**

 

------



## **HashMap이란?**

HashMap은 Map인터페이스에 속해있는 컬렉션입니다

Map 인터페이스의 기본 기능들을 전부 구현할 수 있습니다

데이터들은 모두 (키, 값)의 1:1 구조로 되어있는 Entry로 되어있습니다



![img](https://blog.kakaocdn.net/dn/ylAYE/btq5FUHLOY7/M3azBrXdekSWKaAEf1f7oK/img.png)



같은 키의 값을 삽입하려고하면 해당 키의 값이 변경이 됩니다

키는 고유한 속성이지만 값은 고유한 속성이 아닙니다

키는 중복이 되지 않지만 값은 중복이 될 수 있습니다

다른 특징으로는 HashTable과 유사하지만 동기화가 되지 않고 Null값도 저장이 가능합니다

 

------



## **HashMap 선언하기**

```
import java.util.HashMap;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap hm = new HashMap(); // 타입 설정x Object 입력
		HashMap<Integer, Integer> i = new HashMap<>(); // Integer, Integer 타입 설정
		HashMap<Integer, Integer> i2 = new HashMap<>(i); // i의 값을 i2에 카피
		HashMap<Integer, Integer> i3 = new HashMap<>(10); // 초기용량 지정
		HashMap<Integer, Integer> i4 = new HashMap<>() {{ // 변수 선언 + 초기값 지정
			put(1, 100);
			put(2, 200);
		}};		
		
		HashMap<String, String> str = new HashMap<String, String>(); // String, String 타입 설정
		HashMap<Character, Character> ch = new HashMap<Character, Character>(); // Char, Char 타입 설정
	}
}
```

HashMap을 선언하는 방법은 여러 가지가 있습니다

HashMap은 Key, Value 2개의 값을 가지고 있으므로 타입을 선언하려면 두 개의 타입을 선언해야합니다

HashMap<타입, 타입> 변수명 = new HashMap<타입, 타입>(); 로 선언을 해줍니다

나머지 사용방법들은 위의 예제와 주석을 참고바랍니다

 

------



## **HashMap 값 추가하기**

```
import java.util.HashMap;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap<String, String> hm = new HashMap<String, String>(); // HashMap 선언
		
		// 값 추가
		hm.put("1", "Hello1");
		hm.put("2", "World2");
		hm.put("3", "Hello3");
		hm.put("4", "World4");
		hm.put("2", "WorldWorld2");
		
		System.out.print(hm); // 결과 출력
	}
}
```

HashMap의 값을 추가하는 방법은 put(Key, Value)를 사용합니다

들어가는 타입은 변수를 선언할 당시의 타입으로 맞춰서 입력해줘야합니다

만약 같은 키 값의 데이터를 put하면 기존의 Value가 나중에 넣은 Value로 변경이 됩니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bz26w1/btq5IuaHlFC/hE8KEJkBSpW3PFJL9247l1/img.png)



 

------



## **HashMap 값 삭제하기**

```
import java.util.HashMap;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap<String, String> hm = new HashMap<String, String>(); // HashMap 선언
		
		// 값 추가
		hm.put("1", "Hello1");
		hm.put("2", "World2");
		hm.put("3", "Hello3");
		hm.put("4", "World4");
		
		System.out.println(hm); // 결과 출력

		hm.remove("3");
		System.out.println(hm); // 결과 출력

		hm.clear();
		System.out.println(hm); // 결과 출력
	}
}
```

HashMap의 값을 삭제하는 방법은 remove(Key) 메서드를 사용하여 해당하는 Key를 삭제할 수 있습니다

clear()메서드를 사용하면 HashMap의 모든 키 값을 삭제합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cRhm7I/btq5LJ1jvD4/FYND1zdKkxVGNP3zheyEgK/img.png)



 

------



 

 

## **HashMap 크기 구하기**

```
import java.util.HashMap;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap<String, String> hm = new HashMap<String, String>(); // HashMap 선언
		
		// 값 추가
		hm.put("1", "Hello1");
		hm.put("2", "World2");
		hm.put("3", "Hello3");
		hm.put("4", "World4");
		
		System.out.println(hm); // 결과 출력
		System.out.println("Size : " + hm.size()); 
	}
}
```

HashMap의 크기를 구하는 방법은 size() 메서드를 사용합니다

HashMap안의 키의 갯수를 출력해줍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bHqGLO/btq5AP8kPCe/VCMlHL2SYEp3FQoKqPtdNk/img.png)



 

------



## **HashMap 값 출력하기**

HashMap의 값을 출력하는 방법은 여러가지가 있는데 그 중에서 두 가지 방법을 설명하겠습니다

향상된for문과 Iterator클래스를 사용한 방식입니다

첫 번째 향상된for문을 사용한 방법입니다

```
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap<String, String> hm = new HashMap<String, String>(); // HashMap 선언
		
		// 값 추가
		hm.put("1", "Hello1");
		hm.put("2", "World2");
		hm.put("3", "Hello3");
		hm.put("4", "World4");
		
		for(Map.Entry<String, String> e : hm.entrySet())
			System.out.println("Key : " + e.getKey() + ", Value : " + e.getValue());
	}
}
```

for(Map.Entry<타입, 타입> 변수명 : entrySet()) 을 사용하여 HashMap을 반복문을 실행합니다

이어서 e.getKey(), e.getValue() 메서드를 차례대로 사용하여 HashMap의 Key값과 Value값을 가져올 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/oh0SM/btq5HM42W5v/Qkhui6owUucCyNJ2OhBia1/img.png)



 

두 번째는 Iterator방식을 사용한 방법입니다

```
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Map.Entry;

public class HashMapDemo {
	public static void main(String[] args)  {
		HashMap<String, String> hm = new HashMap<String, String>(); // HashMap 선언
		
		// 값 추가
		hm.put("1", "Hello1");
		hm.put("2", "World2");
		hm.put("3", "Hello3");
		hm.put("4", "World4");
		
		Iterator<Entry<String, String>> iter = hm.entrySet().iterator();
		while(iter.hasNext())
		{
			Map.Entry<String, String> entry = iter.next();
			System.out.println("Key : " + entry.getKey() + ", Value : " + entry.getValue());
		}
		System.out.println("-----------------------------");

		Iterator<String> iter2 = hm.keySet().iterator();
		while(iter2.hasNext())
		{
			String key = iter2.next();
			System.out.println("Key : " + key + ", Value : " + hm.get(key));
		}
	}
}
```

Iterator를 Entry로 선언하여 위의 향상된for문처럼 getKey(), getValue() 방식으로 가져옵니다

Iterator를 String으로 선언하여 키 값을 순차적으로 가져온 후 HashMap에 get메서드를 사용하여 Value를 가져옵니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/8FmMg/btq5G2TiSJE/wuhxiVvkkG4YIrIOTGZ7t1/img.png)

