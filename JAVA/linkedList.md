LinkedList에 대해서 알아보겠습니다

 

> **목차**
>
> **[LinkedList란?](#text1)**
> **[LinkedList 선언하기](#text2)**
> **[LinkedList 값 추가하기](#text3)**
> **[LinkedList 값 변경하기](#text4)**
> **[LinkedList 값 삭제하기](#text5)**
> **[LinkedList 크기 구하기](#text6)**
> **[LinkedList 값 출력하기](#text7)**
> **[LinkedList 값 검색하기](#text8)**

------



## **LinkedList란?**

LinkedList란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다

이 클래스는 데이터가 연속된 위치에 저장되지 않고 모든 데이터가 데이터 부분과 주소 부분을 별도로 가지고 있습니다

데이터는 포인터와 주소를 사용하여 연결합니다

각 데이터는 노드라 불리며 배열에서 자주 삽입, 삭제가 이루어지는 경우 용이하여 ArrayList보다 선호됩니다

하지만 ArrayList보다 검색에 있어서는 느리다는 단점이 있습니다



![img](https://blog.kakaocdn.net/dn/bygm8v/btq5lxMb2f7/UookpU9dnl1uKNZs6i4Bu0/img.png)



위의 사진처럼 LinkedList는 데이터부분과 주소부분이 나눠져있어서 선으로 연결된 형태로 이어져있습니다

 

------



## **LinkedList 선언하기**

```
LinkedList li = new LinkedList(); // 타입 설정x Object로 입력
LinkedList<LinkedListDemo> demo = new LinkedList<LinkedListDemo>(); // List타입을 LinkedListDemo
LinkedList<Integer> i = new LinkedList<Integer>(); // int 타입으로 선언
LinkedList<Integer> i2 = new LinkedList<>(); // 타입 선언 생략도 가능
LinkedList<Integer> i3 = new LinkedList<Integer>(Arrays.asList(1, 2, 3)); // 초기 값 세팅
		
LinkedList<String> s = new LinkedList<String>(); // String 타입 사용
LinkedList<Character> ch = new LinkedList<Character>(); // Char 타입 사용
```

LinkedList의 선언방법입니다

다양한 형태로 타입을 세팅하여 선언할 수 있습니다

Class, Integer, String, Character 등의 타입으로 선언이 가능합니다

 

------



## **LinkedList 값 추가하기**

```
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		ll.add("Hello");
		ll.add("Hello");
		ll.add(1, "World");
		
		System.out.print(ll);
	}
}
```

LinkedList의 값을 추가하기 위해서는 add() 메서드를 사용합니다

add() 메서드의 사용방법은 두 가지가 있습니다

add(Object) : 기본적으로 add를 사용하여 추가하면 LinkedList의 마지막에 데이터를 추가합니다

add(int Index, Object) : LinkedList의 Index에 데이터를 추가합니다



위의 코드를 실행하면 ll.add(1, "World");를 하여 "Hello"와 "Hello" 사이에 "World"가 추가된 것을 확인할 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/JbYZT/btq5gvWgTv0/vdg0XWaJpz65deuYXb2KV1/img.png)



 

------



## **LinkedList 값 변경하기**

```
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		ll.add("Hello");
		ll.add("Hello");
		ll.add(1, "World");
		
		System.out.println(ll);
		
		ll.set(1, "Hello");

		System.out.println(ll);
	}
}
```

LinkedList의 값을 변경하는 방법은 set() 메서드를 사용합니다

값을 바꾸는 것이기 때문에 값을 바꾸려면 Index를 알아야 변경이 가능합니다

set(int Index, Object)로 변경할 수 있습니다

위의 예제를 실행하면 "Hello", "World", "Hello"를 set(1, "Hello")를 사용하여

"Hello", "Hello", "Hello"로 변경합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/c2Bu2s/btq5lzpRRGc/YkGselIXz3nkkkZTD31G81/img.png)



 

------



## **LinkedList 값 삭제하기**

```
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		System.out.println(ll);

		ll.removeFirst(); // 첫 번째 데이터 삭제
		ll.removeLast(); // 마지막 데이터 삭제

		System.out.println(ll);
		
		ll.remove(); // remove로 삭제하면 첫 번째 데이터를 삭제

		System.out.println(ll);

		ll.remove(2); // Index순번의 데이터를 삭제

		System.out.println(ll);

		ll.clear(); // List의 모든 데이터를 삭제
		
		System.out.println(ll);
	}
}
```

값을 제거하는 방법에는 여러 가지가 있습니다

removeFirst(), removeLast() : LinkedList에서 첫 번째와 마지막 데이터를 삭제합니다

remove() : 첫 번째 데이터를 삭제합니다

remove(int Index) : Index 위치의 데이터를 삭제

clear() : List의 모든 데이터를 삭제 -> removeAll(LinkedList)로도 모든 데이터 삭제가 가능합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bl1vZy/btq5nfrcc6U/oiYm9KGam0kl3Wy4pGvrb1/img.png)



 

------



 

 

## **LinkedList 크기 구하기**

```
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		System.out.println(ll);
		System.out.print(ll.size());
	}
}
```

LinkedList의 크기는 size() 메소드를 사용하여 구할 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bfyLsV/btq5c5D8zox/jUvYYP0n1siREdeAVWLbO0/img.png)



 

------



## **LinkedList 값 출력하기**

```
import java.util.Iterator;
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		/* get(i) 메서드를 사용하여 값 출력 */
		for(int i = 0; i < ll.size(); i++)
			System.out.print(ll.get(i) + " ");
		
		System.out.println();

		/* 향상된for문을 사용하여 값 출력 */
		for(String str : ll)
			System.out.print(str + " ");
		
		System.out.println();

		/* Iterator를 사용하여 값 출력 */
		Iterator iter = ll.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

LinkedList의 결과를 출력하는 메서드는 get(int Index)가 있습니다

Index에 해당하는 데이터를 출력합니다

모든 데이터를 출력하고 싶다면 for문을 사용하여 get(int Index)를 반복호출하면 됩니다

추가로 향상된 for문과 Iterator를 사용하여 값을 출력할 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bZDnMW/btq5hZpaUXu/YhX4ZTOipRFPJAqY5YkWck/img.png)



 

------



## **LinkedList 값 검색하기**

```
import java.util.LinkedList;

public class LinkedListDemo {
	public static void main(String[] args)  {
		LinkedList<String> ll = new LinkedList<String>();
		
		/* 값을 추가한다 */
		ll.add("Hello");
		ll.add("World");
		ll.add("Hello");
		ll.add("World");
		
		System.out.println("값 검색(contains) : " + ll.contains("Hello"));
		System.out.println("값 검색(IndexOf) : " + ll.indexOf("Hello"));
	}
}
```

값을 찾는 방법에는 contains()와 indexOf()가 있습니다

두 개는 비슷하지만 용도가 다릅니다

contains(Object) : LinkedList에서 값이 있는지 없는지 여부만 판단합니다

 값이 있다면 true, 없다면 false를 리턴합니다

indexOf(Object) : LinkedList에서 값의 Index를 찾아줍니다

 만약 값이 없다면 -1을 리턴합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cl8wY6/btq5hkmx2jE/2d1Jns3fwEBEK9iqimTO00/img.png)