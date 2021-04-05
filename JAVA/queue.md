Queue에 대해서 알아보겠습니다

 

> **목차**
>
> **[Queue란?](#text1)**
> **[Queue 선언하기](#text2)**
> **[Queue 값 추가하기](#text3)**
> **[Queue 값 삭제하기](#text4)**
> **[Queue 크기 구하기](#text5)**
> **[Queue 값 출력하기](#text6)**

------



## **Queue란?**

Queue란 Collection 프레임워크의 일부이며 java.util 패키지에 소속되어 있습니다



![img](https://blog.kakaocdn.net/dn/4xNC4/btq5pEMF0sO/Hz54KOz8oU8QwR8uCqyIMK/img.png)



Queue는 사전적으로 "줄을 서다"를 의미합니다

줄을 서서 기다린다는 것처럼 먼저 들어오면 데이터가 먼저 나가는 형식입니다

일명 FIFO(FirstInFirstOut) 방식입니다

반대로 Stack은 LIFO방식이라 두 개가 많이 비교됩니다

 

위의 그림에서 볼 수 있지만 큐는 앞과 뒤가 다른 역할을 수행합니다

큐의 앞 부분은 front는 삭제 연산만 수행

큐의 뒷 부분은 rear는 삽입 연산만 수행합니다

보통 컴퓨터 버퍼에서 주로 사용, 여러 개가 한꺼번에 입력이 들어올 때 대기열을 만들어 순차적으로 처리할 때 사용이 됩니다

아래에서는 Queue의 사용법에 대해서 알아보겠습니다

 

------



## **Queue 선언하기**

```
Queue queue = new LinkedList(); // 타입 설정x Object로 입력
Queue<QueueDemo> demo = new LinkedList<QueueDemo>(); // 타입을 QueueDemo클래스로 설정
		
Queue<Integer> i = new LinkedList<Integer>(); // Integer타입으로 선언
Queue<Integer> i2 = new LinkedList<>(); // new부분 타입 생략 가능
Queue<Integer> i3 = new LinkedList<Integer>(Arrays.asList(1, 2, 3)); // 선언과 동시에 초기값 세팅
		
Queue<String> str = new LinkedList<String>(); // String타입 선언
Queue<Character> ch = new LinkedList<Character>(); // Character타입 선언
```

자바에서의 Queue의 선언방법입니다

Queue로 생성을 하면 안 되고 LinkedList로 생성을 해야합니다

맨 처음 할 때 Queue로 생성하는데 계속 에러 떠서 당황;;

LinkedList를 사용하기 때문에 Queue와 LinkedList를 모두 import해야합니다

위의 예제처럼 다양한 타입으로 Queue는 선언이 가능합니다

 

------



## **Queue 값 추가하기**

```
import java.util.LinkedList;
import java.util.Queue;

public class QueueDemo {
	public static void main(String[] args)  {
		Queue<String> que = new LinkedList<String>();
		
		// 값 추가
		que.add("Hello");
		que.add("World");
		
		System.out.print(que); // 결과 출력
	}
}
```

Queue의 값을 추가하는 방법은 add() 메서드를 사용하여 추가합니다

add(Object)로 값을 추가합니다

값은 뒤에서부터 차례대로 들어옵니다

다른 Collection의 경우에는 중간에 값을 추가하는 것이 가능하지만 Queue는 데이터는 맨 뒤로만 들어올 수 있습니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cOSy0Y/btq5rP1iwVN/MBwqDcRk1Jkx2AkYTe5B90/img.png)



 

------



## **Queue 값 삭제하기**

```
import java.util.LinkedList;
import java.util.Queue;

public class QueueDemo {
	public static void main(String[] args)  {
		Queue<String> que = new LinkedList<String>();
		
		// 값 추가
		que.add("Hello");
		que.add("World");
		que.add("Hello");
		que.add("Hello");
		que.add("World");
		
		System.out.println(que); // 결과 출력 -> [Hello, World, Hello, Hello, World]
		
		que.poll(); // 맨 앞의 값 삭제

		System.out.println(que); // 결과 출력 -> [World, Hello, Hello, World]
		
		que.remove(); // 맨 앞의 값 삭제

		System.out.println(que); // 결과 출력 -> [Hello, Hello, World]

		que.remove("Hello"); // 해당하는 값 삭제

		System.out.println(que); // 결과 출력 -> [Hello, World]

		que.clear(); // Index의 값 삭제

		System.out.println(que); // 결과 출력 -> []
	}
}
```

Queue의 값을 삭제하는 방법은 여러 가지가 있습니다

기본적으로 제거하는 방법은 poll()과 remove() 메서드가 있습니다

poll()과 remove()의 기능은 같지만

poll()에서는 대기열이 비어있다면 null을 반환합니다

remove()에서는 대기열이 비어있으면 NoSuchElement 에러를 반환합니다

 

remove(Object) 메서드를 사용하면 Object에 해당하는 값을 삭제합니다

만약 두 개가 있다면 더 앞에 있는 값을 삭제합니다

 

clear() 메서드는 Queue의 값을 모두 삭제하고 초기화합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/cWCYG7/btq5rQeRm3M/sRx2HcKhotBWQ9KkcHMfjK/img.png)



 

------



 

 

## **Queue 크기 구하기**

```
import java.util.LinkedList;
import java.util.Queue;

public class QueueDemo {
	public static void main(String[] args)  {
		Queue<String> que = new LinkedList<String>();
		
		// 값 추가
		que.add("Hello");
		que.add("World");
		que.add("Hello");
		que.add("Hello");
		que.add("World");
		
		System.out.println("Queue의 크기 : " + que.size()); 
	}
}
```

Size() 메서드를 사용하면 Queue의 크기를 구합니다

Queue 안의 갯수를 구해서 출력해줍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/ci8Q4i/btq5sVAgb8i/AZ57n9k1wufcnyVethyTkk/img.png)



 

------



## **Queue 값 출력하기**

```
import java.util.Iterator;
import java.util.LinkedList;
import java.util.Queue;

public class QueueDemo {
	public static void main(String[] args)  {
		Queue<String> que = new LinkedList<String>();
		
		// 값 추가
		que.add("Hello");
		que.add("World");
		que.add("Hello");
		que.add("Hello");
		que.add("World");
		
		System.out.println("첫 번째 값 출력 : " + que.peek());
				
		/* Iterator 클래스를 사용하여 값 출력 */
		Iterator iter = que.iterator();
		
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

Queue에서 값을 출력하는 방법은 peek() 메서드가 있습니다

peek() 메서드를 사용하면 맨 처음 넣은 값을 확인할 수 있습니다

모든 값을 출력하고 싶다면 Iterator 클래스를 사용하여 출력해야합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/oDHt6/btq5qqHBJxG/55Nri4qf6VTSD1FK7ow6kk/img.png)

