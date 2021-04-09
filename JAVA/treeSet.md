TreeSet에 대해서 알아보겠습니다

 

> **목차**
>
> **[TreeSet이란?](#text1)**
> **[TreeSet 선언하기](#text2)**
> **[TreeSet 값 추가하기](#text3)**
> **[TreeSet 값 삭제하기](#text4)**
> **[TreeSet 크기 구하기](#text5)**
> **[TreeSet 값 출력하기](#text6)**

 

 

------



## **TreeSet이란?**

자바의 SortedSet 인터페이스 중 하나입니다

HashSet과 비슷한 구조를 가져서 중복 데이터를 저장하지 않고 저장 순서를 유지하지 않는다는 성질을 가집니다

HashSet과 다른 점은 TreeSet은 이진 탐색 트리(BinarySearchTree) 구조로 되어있습니다

이진 탐색 트리에서 검색능력을 더 향상시킨 레드-블랙-트리로 구현되어 있습니다



![img](https://blog.kakaocdn.net/dn/ma4v6/btq5FUggY84/GXDP0TxkCwiJ3tcCtzJqRK/img.png)



 

------



## **TreeSet 선언하기**

```
import java.util.Collections;
import java.util.TreeSet;

public class TreeSetDemo {
	public static void main(String[] args)  {
		// Integer 타입으로 오름차순 정렬
		TreeSet<Integer> ts = new TreeSet<>();
		
		// Integer 타입으로 내림차순 정렬
		TreeSet<Integer> ts2 = new TreeSet<>(Collections.reverseOrder());

		// String 타입으로 오름차순 정렬
		TreeSet<String> ts3 = new TreeSet<>();

		// String 타입으로 내림차순 정렬
		TreeSet<String> ts4 = new TreeSet<>(Collections.reverseOrder());
	}
}
```

TreeSet의 생성방법입니다

생성을 위해서는 java.util.TreeSet 클래스를 import합니다

TreeSet<타입> 변수명 = new TreeSet<>(); 선언하면 자동으로 오름차순 정렬이 되는 TreeSet을 선언합니다

Collections.reverseOrder() 메서드를 사용하면 내림차순 정렬로 바꿀 수 있습니다



Collections를 사용하기 위해서는 java.util.Collections를 import해야합니다

 

------



## **TreeSet 값 추가하기**

```
import java.util.TreeSet;

public class TreeSetDemo {
	public static void main(String[] args)  {
		// TreeSet 선언
		TreeSet<Integer> ts = new TreeSet<>();
		
		// 값 추가
		ts.add(5);
		ts.add(4);
		ts.add(3);
		ts.add(2);
		ts.add(1);
		
		System.out.println(ts); // 결과 출력
	}
}
```

TreeSet의 값을 추가하는 방법입니다

add(Object) 메서드를 사용하여 값을 추가할 수 있습니다

TreeSet은 기본적으로 정렬을 해주기때문에 결과를 출력하면 정렬된 값으로 출력이 됩니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bKONYg/btq5G0NTypI/pXoj9iaiuD1LevwPIUBYi1/img.png)



 

------



## **TreeSet 값 삭제하기**

```
import java.util.TreeSet;

public class TreeSetDemo {
	public static void main(String[] args)  {
		// TreeSet 선언
		TreeSet<Integer> ts = new TreeSet<>();
		
		// 값 추가
		ts.add(5);
		ts.add(4);
		ts.add(3);
		ts.add(2);
		ts.add(1);
		
		System.out.println(ts); // 결과 출력
		
		ts.remove(4); // 4 삭제
		System.out.println(ts); // 결과 출력

		ts.pollFirst(); // 첫 데이터 삭제
		System.out.println(ts); // 결과 출력

		ts.pollLast(); // 마지막 데이터 삭제
		System.out.println(ts); // 결과 출력

		ts.clear(); // 전체삭제
		System.out.println(ts); // 결과 출력
	}
}
```

TreeSet에서 값을 삭제하는 방법입니다

remove(Object)를 사용하면 Object에 대한 값만 삭제합니다

Object가 없는 경우 false를 리턴해줍니다

pollFirst(), pollLast() 메서드를 사용하면 맨 처음 데이터와 맨 마지막 데이터를 삭제해줍니다

값을 모두 삭제하고 싶은 경우 clear() 메서드를 사용합니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bKDP2M/btq5FVTSMp4/QrCTcjePYKMyT8Z1uXf7i1/img.png)



 

------



 

 

## **TreeSet 크기 구하기**

```
import java.util.TreeSet;

public class TreeSetDemo {
	public static void main(String[] args)  {
		// TreeSet 선언
		TreeSet<Integer> ts = new TreeSet<>();
		
		// 값 추가
		ts.add(5);
		ts.add(4);
		ts.add(3);
		ts.add(2);
		ts.add(1);
		
		System.out.println(ts); // 결과 출력
		System.out.println("TreeSet의 크기는 " + ts.size());
	}
}
```

TreeSet의 크기를 구하는 방법은 size() 메서드를 사용하여 구할 수 있습니다

TreeSet에 있는 값의 갯수를 출력해줍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/qvuyo/btq5CBIR283/z0YGCeR0ZN6A6owluCvKvk/img.png)



 

------



## **TreeSet 값 출력하기**

```
import java.util.Iterator;
import java.util.TreeSet;

public class TreeSetDemo {
	public static void main(String[] args)  {
		// TreeSet 선언
		TreeSet<Integer> ts = new TreeSet<>();
		
		// 값 추가
		ts.add(5);
		ts.add(4);
		ts.add(3);
		ts.add(2);
		ts.add(1);
		
		/* 향상된 for문을 사용하여 출력 */
		for(Integer str : ts)
			System.out.print(str + " ");
		
		System.out.println();
		
		// Iterator를 사용하여 출력 */
		Iterator iter = ts.iterator();
		while(iter.hasNext())
			System.out.print(iter.next() + " ");
	}
}
```

TreeSet의 값을 출력하는 방법입니다

향상된 for문을 사용하여 출력하거나 Iterator클래스를 사용하여 출력하는 방법입니다

위의 코드를 참고바랍니다

 

**결과**



![img](https://blog.kakaocdn.net/dn/bFAb5w/btq5HCzioU1/RuDh7gjASvEaXAnDpcSbDk/img.png)