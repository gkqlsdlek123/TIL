Stack에 대해서 알아보겠습니다

> **목차**
>
> **[Stack란?](#text1)**
> **[Stack 선언하기](#text2)**
> **[Stack 값 추가하기](#text3)**
> **[Stack 값 삭제하기](#text4)**
> **[Stack 크기 구하기](#text5)**
> **[Stack 값 출력하기](#text6)**
> **[Stack 값 검색하기](#text7)**

 

------



## **Stack란?**

사전적 의미로는 '쌓다', '더미'라는 뜻을 가지고 있습니다

또한 Collection 프레임워크의 일부이며 java.util 패키에서 소속되어 있습니다

Stack의 가장 큰 특징은 후입선출(LIFO : Last In First Out)입니다



![img](https://blog.kakaocdn.net/dn/bn1hrn/btq5orNbcHz/B0kPySPDsaSj6b4tfQOkck/img.png)



위와 같은 원리로 동작된다고 보시면 됩니다

 

------



## **Stack 선언하기**

```
Stack st = new Stack(); // 타입 설정x Object로 선언
Stack<StackDemo> demo = new Stack<StackDemo>(); // class타입으로 선언
Stack<Integer> i = new Stack<Integer>(); // Integer타입 선언
Stack<Integer> i2 = new Stack<>(); // 뒤의 타입 생략 가능
		
Stack<String> s = new Stack<String>(); // String타입 선언
Stack<Character> ch = new Stack<Character>(); // Char타입 선언
```

Stack의 선언 방법입니다

Stack<타입> 변수명 = new Stack<타입>(); 으로 선언합니다

타입 선언의 생략이 가능하지만 처음 들어간 동일한 타입으로 입력을 계속하지 않으면 타입에러가 발생하므로 타입을 명확하게 선언하는게 좋습니다

타입은 Integer, String, Character 등 여러가지 형태로 선언이 가능합니다

 

------



## **Stack 값 추가하기**

```
import java.util.Stack;

public class StackDemo {
	public static void main(String[] args)  {
		Stack<String> s = new Stack<String>();
		
		// Stack 값 추가
		s.push("Hello");
		s.push("World");
		
		System.out.print(s); // 결과 출력
	}
}
```

push() 메서드를 사용하여 값을 추가할 수 있습니다

다른 배열들과는 다르게 LIFO 방식을 사용하므로 도중에 데이터를 삽입하는 것이 불가능합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/WaA3Y/btq5tiuqhw6/TeRvjbW9y8ihAE9qqVDs70/img.png)



 

------



## **Stack 값 삭제하기**

```
import java.util.Stack;

public class StackDemo {
	public static void main(String[] args)  {
		Stack<String> s = new Stack<String>();
		
		// Stack 값 추가
		s.push("Hello");
		s.push("World");
		
		System.out.println(s); // 결과 출력
		
		s.pop(); // Stack 값 제거

		System.out.println(s); // 결과 출력
		
		s.clear(); // Stack 값 전체 제거

		System.out.println(s); // 결과 출력
	}
}
```

pop() 메서드를 사용하면 Stack에서 값을 삭제합니다

값은 LIFO를 따라서 맨 마지막에 Input한 값을 삭제합니다

clear() 메서드를 사용하면 Stack의 모든 값을 삭제합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/0jVYK/btq5sR5iHkO/RmXX3CpBMXDwjgWbvT7xKK/img.png)



 

------



 

 

## **Stack 크기 구하기**

```
import java.util.Stack;

public class StackDemo {
	public static void main(String[] args)  {
		Stack<String> s = new Stack<String>();
		
		// Stack 값 추가
		s.push("Hello");
		s.push("World");
		
		System.out.println("Size : " + s.size()); // 결과 출력
	}
}
```

Stack의 크기는 size() 메서드를 사용하여 구할 수 있습니다

Stack안에 있는 값의 갯수를 출력합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/crAEqq/btq5sU14mRQ/UCDowfgI0CG6LpdiG6Pws0/img.png)



 

------



## **Stack 값 출력하기**

```
import java.util.Iterator;
import java.util.Stack;

public class StackDemo {
	public static void main(String[] args)  {
		Stack<String> s = new Stack<String>();
		
		// Stack 값 추가
		s.push("Hello");
		s.push("World");
		
		// firstElement(), lastElement(), peek()을 사용 -> 처음, 마지막, 마지막 값을 불러온다
		System.out.println("처음 값 : " + s.firstElement());
		System.out.println("마지막 값 : " + s.lastElement());
		System.out.println("마지막 값 : " + s.peek());
		
		// get(i) 메서드를 사용하여 Stack의 Index 값을 출력
		for(int i = 0; i < s.size(); i++)
			System.out.print(s.get(i) + " ");
		
		System.out.println();
		
		// 향상된for문을 사용하여 Stack의 값을 출력
		for(String str: s)
			System.out.print(str + " ");
		
		// Iterator를 사용하여 Stack의 값을 출력 
		Iterator iter = s.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

firstElement() 메서드를 사용하여 Stack의 맨 처음 Input한 값을 찾을 수 있습니다

lastElement(), peek() 메서드를 사용하여 Stack의 맨 마지막에 Input한 값을 꺼내올 수 있습니다

lastElement()와 peek()의 구조적인 차이점은 아직 저도 잘 모르겠습니다 ㅠ

Stack이 만약 Empty() 상태라면

peek에서는 **EmptyStackException** 에러를 던져줍니다 

lastElement에서는 **NoSuchElementException** 에러를 던져줍니다

 

get(int Index) 메서드를 사용하면 해당 Index의 값을 찾아 호출해줍니다

for문, 향상된 반복문, Iterator클래스를 사용하여 Stack의 값을 모두 표시해줄 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cHJ8W6/btq5or7UJIA/KcwxTmtoLXa7GKkUeeW2rK/img.png)



 

------



## **Stack 값 검색하기**

```
import java.util.Stack;

public class StackDemo {
	public static void main(String[] args)  {
		Stack<String> s = new Stack<String>();
		
		// Stack 값 추가
		s.push("Hello");
		s.push("World");
		s.push("Hello");
		s.push("World");
		
		System.out.println("값 검색(contains) : " + s.contains("Hello"));
		System.out.println("값 검색(indexOf) : " + s.indexOf("Hello"));
	}
}
```

Stack의 값 검색방법입니다

contains()와 indexOf()가 대표적으로 있습니다

contains(Obejct) : 메서드를 호출하면 값의 여부를 찾아 true, false를 반환합니다

indexOf(int Index) : 메서드를 호출하면 값을 위치 Index를 반환합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/EYVE0/btq5s5h29SM/XKGWXPK5NnuS3mkCYprup0/img.png)

