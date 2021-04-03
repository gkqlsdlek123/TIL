Vector에 대해서 알아보겠습니다

 

> **목차**
>
> **[Vector란?](#text1)**
> **[Vector 선언하기](#text2)**
> **[Vector 값 추가하기](#text3)**
> **[Vector 값 변경하기](#text4)**
> **[Vector 값 제거하기](#text5)**
> **[Vector 크기 구하기](#text6)**
> **[Vector 값 출력하기](#text7)**

 

------



## **Vector란?**

Vector란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다

ArrayList와 동일한 구조를 가지며 배열의 크기가 늘어나고, 줄어듬에 따라서 자동으로 크기가 조절이 됩니다



![img](https://blog.kakaocdn.net/dn/94FMN/btq5g5wyWyV/JJ6VO6WAEd5nFtrn7dp3dk/img.gif)



Vector의 특이한 점이라면 항상 동기화되어있고 Collection 프레임워크에 없는 메서드들을 사용이 가능합니다

하지만 동기화라는 특징이 있어 스레드가 아닌 환경에서는 거의 사용이 되지 않습니다

그리고 항상 동기화되므로 스레드 환경에서의 안정성은 높지만 ArrayList와 비교하여 추가, 검색, 삭제의 성능은 떨어지는 단점이 있습니다

 

------



## **Vector 선언하기**

```
Vector V = new Vector(); // 타입 설정x Object로 사용 
Vector<VectorDemo> demo = new Vector<VectorDemo>(); // 타입설정 VectorDemo 객체로 선언 
Vector<Integer> i = new Vector<Integer>(); // int 타입으로 선언 
Vector<Integer> i2 = new Vector<>(); // 타입 선언 생략
Vector<Integer> i3 = new Vector<Integer>(10); // 초기 용량 세팅 
Vector<Integer> i4 = new Vector<Integer>(Arrays.asList(1, 2, 3, 4)); // 초기 값 세팅 
		
Vector<String> s = new Vector<String>(); // String 타입 사용 
Vector<Character> ch = new Vector<Character>(); // char 타입 사용
```

Vector의 선언방법입니다

위의 예제와 같이 Class, Integer, String, Character 등의 다양한 타입으로 선언이 가능합니다

 

------



## **Vector 값 추가하기**

```
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("Hello");
		V.add(1, "World");
		V.add(null);
		
		System.out.print(V);
	}
}
```

Vector의 값을 추가하기 위해서는 add() 메서드를 사용합니다

add() 메서드의 사용방법은 두 가지가 있습니다

add(Object) : 기본적으로 add를 사용하여 추가하면 Vector의 마지막에 데이터를 추가합니다

add(int Index, Object) : Vextor의 Index위치에 데이터를 추가합니다

참고사항으로 Vector는 null을 허용하여 null값도 추가할 수 있습니다

위의 코드를 실행하면 V.add(1, "World");를 하여 "Hello"와 "Hello" 사이에 "World"가 추가된 것을 확인할 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bu5wKP/btq5fV88AjN/6eLwOoabHqT8QG6pnA20K1/img.png)



 

------



## **Vector 값 변경하기**

```
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("Hello");
		V.add(1, "World");
		
		System.out.println(V);
		
		V.set(1, "Hello");

		System.out.println(V);
	}
}
```

Vector의 값을 변경하는 방법은 set() 메서드를 사용합니다

값을 바꾸려면 조건이 필요한데 Index를 알아야 원하는 값을 변경이 가능합니다

set(int Index, Object)로 변경할 수 있습니다

위의 예제를 실행하면 "Hello", "World", "Hello"를 set(1, "Hello")를 사용하여

"Hello", "Hello", "Hello"로 변경합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bOB3d8/btq5g54tRhS/COEBDolO5YLG6OG9AJBYp1/img.png)



 

------



## **Vector 값 제거하기**

```
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("World");
		V.add("Hello");
		V.add("World");
		
		System.out.println(V);
		
		V.remove(1); // Index 1의 값 제거

		System.out.println(V);

		V.removeAllElements(); // 모든 데이터 제거 

		System.out.println(V);

		V.add("Hello");
		V.add("World");
		V.clear(); // 모든 데이터 제거

		System.out.println(V);
	}
}
```

Vector의 값을 삭제하는 방법입니다

원하는 값을 삭제하는 방법은 remove(int Index)를 사용하여 삭제합니다

값을 한꺼번에 삭제하려면 removeAllElements(), clear() 메서드를 사용하여 삭제할 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/d9Sluk/btq5lylt3LR/B0UO0PgHVt5il0HtduF4K0/img.png)



 

------



 

 

## **Vector 크기 구하기**

```
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector V = new Vector(); 
		
		V.add("Hello");
		V.add("World");
		V.add("Hello");
		V.add("World");
		
		System.out.println("Size : " + V.size()); // Vector의 크기 구하기
		System.out.println("Capacity : " + V.capacity()); // Vector의 용량 구하기
	}
}
```

Vector의 크기 및 용량을 구하는 방법입니다

size() 메서드를 사용하면 Vector 데이터의 개수를 구합니다

capacity() 메서드를 사용하면 Vector의 용량을 구합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bBfKrR/btq5naEuhGV/2YtpS6dGqy7ZGhH0YWE5kk/img.png)



 

------



## **Vector 값 출력하기**

```
import java.util.Iterator;
import java.util.Vector;

public class VectorDemo {
	public static void main(String[] args)  {
		Vector<String> V = new Vector<>(); 
		
		V.add("Hello");
		V.add("World");
		V.add("Hello");
		V.add("World");
		
		// get(i)를 사용하여 값 출력
		for(int i = 0; i < V.size(); i++)
			System.out.print(V.get(i) + " ");

		System.out.println();
		
		// 향상된for문을 사용하여 값 출력
		for(String s : V)
			System.out.print(s + " ");

		System.out.println();
		
		// Iterator 사용 값 출력
		Iterator iter = V.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

Vector에서 값을 출력하는 방법은 get() 메서드를 사용합니다

get(int Index)로 원하는 Index의 값을 호출하면 됩니다

다른 방법으로는 향상된for문을 사용하여 배열과 같이 출력하는 방법과

Iterator 클래스를 사용하여 hasNext(), ext() 메서드를 사용하는 방법이 있습니다

위의 예제를 참고바랍니다!

 

**결과**



![img](https://blog.kakaocdn.net/dn/zS6OW/btq5rONOIsS/h39kGc7RrhxADAMbecHLgk/img.png)

