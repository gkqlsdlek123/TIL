Deque와 ArrayDeque에 대해서 알아보겠습니다

 

> **목차**
>
> **[Deque란?](#text1)**
> **[Deque 선언하기](#text2)**
> **[Deque 값 추가하기](#text3)**
> **[Deque 값 삭제하기](#text4)**
> **[Deque 크기 구하기](#text5)**
> **[Deque 값 출력하기](#text6)**

------



## **Deque란?**

Deque란 Double-Ended Queue의 줄임말로 큐의 양쪽에서 데이터를 삽입과 삭제를 할 수 있는 자료구조를 의미합니다



![img](https://blog.kakaocdn.net/dn/cPRHWD/btq5vEkVTi7/AomcAuExOUprd5RsdEgYm0/img.png)



java.util 패키지에 소속되어 있고 Null요소는 사용을 하지 못 합니다

사용하기에 따라서 Stack으로 사용될 때는 Stack보다 빠를 수 있고

대기열에서 사용될 때는 LinkedList보다 빠를 수 있습니다

 

------



## **Deque 선언하기**

```
Deque que = new ArrayDeque(); // 타입 설정x
Deque<DequeDemo> demo = new ArrayDeque<DequeDemo>(); // DequeDemo 클래스 타입으로 선언
Deque<Integer> i = new ArrayDeque<Integer>(); // Integer 타입
Deque<Integer> i2 = new ArrayDeque<>(); // 뒷부분은 생략 가능
Deque<Integer> i3 = new ArrayDeque<Integer>(Arrays.asList(1, 2, 3, 4)); // 선언과 동시에 초기값 세팅
		
Deque<String> s = new ArrayDeque<String>(); // String 타입
Deque<Character> ch = new ArrayDeque<Character>(); // Char 타입
```

Deque를 선언하려면 new Deque()로 선언이 안 됩니다

Deque대신 new ArrayDeque(), new LinkedList() 등으로 선언을 해줘야합니다

선언 방법에는 Integer, String, Character, 클래스 등 다양한 방법으로 선언이 가능합니다

 

------



## **Deque 값 추가하기**

```
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeDemo {
	public static void main(String[] args)  {
		Deque<String> deque = new ArrayDeque<String>(); // Deque 선언
		
		// 값 추가
		deque.addFirst("Hello");
		deque.offerFirst("Hello");
		deque.addLast("World");
		deque.offerLast("World");
		deque.add("Hello");
		
		System.out.print(deque); // 결과 출력
	}
}
```

Deque의 값을 추가하는 메서드는 addFirst(), offerFirst(), addLast(), offerLast(), add() 이렇게 여러 개가 있습니다

차례대로 설명해보겠습니다

addFirst() : Deque의 맨 앞에 데이터를 삽입합니다. 용량을 초과하는 경우 Exception이 발생합니다

offerFirst() : Deque의 맨 앞에 데이터를 삽입합니다. 용량을 초과하는 경우 false를 리턴해줍니다



addLast() : Deque의 맨 뒤에 데이터를 삽입합니다. 용량을 초과하는 경우 Exception이 발생합니다

offerLast() : Deque의 맨 뒤에 데이터를 삽입합니다. 용량을 초과하는 경우 false를 리턴해줍니다

add() : addLast()와 동일합니다. 맨 뒤에 데이터를 삽입하고 용량을 초과하는 경우 Exception이 발생합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bPe7Fc/btq5wvJcv3C/hqsEORFRAZFavV7q9w5vd1/img.png)



 

------



## **Deque 값 삭제하기**

```
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeDemo {
	public static void main(String[] args)  {
		Deque<String> deque = new ArrayDeque<String>(); // Deque 선언
		
		// 값 추가
		deque.add("Hello1");
		deque.add("World2");
		deque.add("Hello3");
		deque.add("World4");	
		deque.add("Hello5");
		deque.add("World6");	
		deque.add("Hello7");
		deque.add("World8");	
						
		System.out.println(deque); // 결과 출력

		deque.removeFirst(); // 첫 번째 삭제
		System.out.println(deque); // 결과 출력

		deque.pollFirst(); // 첫 번째 삭제
		System.out.println(deque); // 결과 출력

		deque.remove(); // 첫 번째 삭제
		System.out.println(deque); // 결과 출력

		deque.poll(); // 첫 번째 삭제
		System.out.println(deque); // 결과 출력

		deque.removeLast(); // 마지막 삭제
		System.out.println(deque); // 결과 출력

		deque.pollLast(); // 마지막 삭제
		System.out.println(deque); // 결과 출력

		deque.remove("World6"); // 원하는 데이터 삭제
		System.out.println(deque); // 결과 출력

		deque.clear(); // 모두 삭제
		System.out.println(deque); // 결과 출력
	}
}
```

Deque의 값을 삭제하는 방법에는 여러가지가 있습니다

첫 번째부터 순서대로 삭제하는 방법입니다

removeFirst(), pollFirst(), remove(), poll() 이 있습니다

remove와 poll차이점은

remove는 deque가 비어있다면 Exception을 출력하지만

poll은 deque가 비어있다면 null을 리턴합니다

 

마지막부터 순서대로 삭제하는 방법은 removeLast(), pollLast()가 있습니다

위의 마찬가지로 remove는 deque가 비어있으면 Exception, poll을 null을 리턴합니다

 

remove(Object) 메서드를 사용하면 deque에서 Object를 찾아 삭제합니다

clear() 메서드를 사용하면 deque의 모든 데이터를 삭제합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/kJl0v/btq5xui1Y61/udS0uztoNs0cerLebNoXJ1/img.png)





------

 

 

 

## **Deque 크기 구하기**

```
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeDemo {
	public static void main(String[] args)  {
		Deque<String> deque = new ArrayDeque<String>(); // Deque 선언
		
		// 값 추가
		deque.add("Hello1");
		deque.add("World2");
		deque.add("Hello3");
		deque.add("World4");	
		deque.add("Hello5");
		deque.add("World6");	
		deque.add("Hello7");
		deque.add("World8");	
						
		System.out.println("Deque의 사이즈 = " + deque.size()); // 결과 출력
	}
}
```

Deque의 크기를 구하는 방법은 size() 메서드로 구할 수 있습니다

Deque안의 데이터의 갯수를 출력해줍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bJzScx/btq5wu4JRBT/rb5u41q58hOQuoOC2e0cLk/img.png)



 

------



## **Deque 값 출력하기**

```
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Iterator;

public class DequeDemo {
	public static void main(String[] args)  {
		Deque<String> deque = new ArrayDeque<String>(); // Deque 선언
		
		// 값 추가
		deque.add("Hello1");
		deque.add("World2");
		deque.add("Hello3");
		deque.add("World4");	
		deque.add("Hello5");
		deque.add("World6");	
		deque.add("Hello7");
		deque.add("World8");	
						
		System.out.println("첫 번째 값 : " + deque.getFirst() + ", " + deque.peekFirst() + ", " + deque.peek());
		System.out.println("마지막 값 : " + deque.getLast() + ", " + deque.peekLast());		

		/* Iterator 클래스를 사용하여 값 출력 */
		Iterator iter = deque.iterator();
		
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

Deque의 값을 출력하는 방법입니다

Deque의 첫 번째 데이터를 가져오는 방법에는 getFirst(), peekFirst(), peek() 메서드가 있습니다

반대로 마지막 데이터를 가져오는 방법에는 getLast(), peekLast() 메서드가 있습니다

Iterator클래스를 사용하여 전체 Deque의 데이터를 출력할 수도 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cEQl4g/btq5BnQ3Dkg/NIJox1YNkdV1Cfvo4zzOq1/img.png)



