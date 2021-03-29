자바의 Collection Framework에서 저장된 요소를 불러오는 방법인 반복자에 대해서 알아보겠습니다

Enumeration, Iterator, ListIterator

 

> **목차**
>
> **[Enumeration란?](#text1)**
> **[Iterator란?](#text2)**
> **[ListIterator란?](#text3)**

 

------



## **Enumeration란?**

Legacy Collection(Vector, Hashtable)의 데이터를 가져오는데 사용되는 클래스입니다.

위의 Vector와 Hashtable에서만 사용이 가능하여 범용적인 클래스는 아닙니다

순방향으로 읽어들이기만 가능합니다

추가하거나 제거하는 작업은 불가능합니다

 

Enumeration의 주요 메서드입니다

| 메서드                    | 설명                                                         |
| ------------------------- | ------------------------------------------------------------ |
| boolean hasMoreElements() | 데이터가 계속해서 있다면 true를 반환, 없다면 false를 반환합니다 |
| Object nextElement()      | 반복해서 다음 데이터를 반환합니다 더 이상 데이터가 없으면 NoSuchElementException 에러가 발생합니다 |

 

위의 주요 메서드를 사용한 예제를 보면서 메서드에 대해서 알아보겠습니다

hasMoreElements() 메서드는 주로 while문과 같이 사용이 됩니다

nextElement() 메서드는 변수에 데이터를 입력하거나 다음 단계의 데이터를 확인할 때 사용됩니다

```
import java.util.Enumeration;
import java.util.Vector;

public class EnumerationDemo {
	public static void main(String[] args)  {	
        // Vector 선언 및 데이터 입력
        Vector v = new Vector();
        for (int i = 0; i < 10; i++)
            v.addElement(i);
        System.out.println(v);
  
        // Enumeration을 Vector로 데이터 초기화
        Enumeration e = v.elements();
  
        // 다음 데이터가 있는지 체크
        while (e.hasMoreElements())
        {
            // 다음 데이터로 이동
            int i = (Integer)e.nextElement();
  
            System.out.print(i + " ");
        }
	}
}
```

 

결과 화면



![img](https://blog.kakaocdn.net/dn/dALZ2D/btq44cQZp0T/goUhF6pPOuU9mij6sSqbgk/img.png)



 

------



 

 

## **Iterator란?**

 반복자로써 Collection Framework의 데이터를 하나씩 검색하는데 사용합니다

Iterator는 모든 Collection 객체에서 사용할 수 있는 범용 클래스입니다

하지만 단방향만 지원을 하고, 새로운 데이터의 교체 및 추가가 불가능합니다

새로 데이터를 교체하거나 추가하려면 다시 Iterator의 값을 초기화해야합니다

 

Iterator의 주요 메서드입니다

| 메서드            | 설명                                                         |
| ----------------- | ------------------------------------------------------------ |
| boolean hasNext() | 데이터가 계속해서 있다면 true를 반환, 없다면 false를 반환합니다 |
| Object next()     | 반복해서 다음 데이터를 반환합니다 더 이상 데이터가 없으면 NoSuchElementException 에러가 발생합니다 |
| void remove()     | 다음 데이터를 제거합니다 이 메서드는 호출 당 한 번만 호출 할 수 있습니다 다음 메서드가 호출되지 않았거나 메서드의 마지막 호출 이후 사용하면 IllegalStateException 에러가 발생합니다 Collection이 제거 작업을 지원하지 않는 경우 UnsupportedOperationException 에러가 발생합니다 |

 

예제는 통해서 Iterator의 주요 메서드에 대해서 알아보겠습니다

hasNext()는 while문과 같이 주로 사용이 됩니다

next()는 변수의 데이터를 입력할 때 다음 단계의 데이터를 읽을 때 사용됩니다

remove() 많이 사용하지 않지만 데이터를 삭제할 때 사용됩니다

```
import java.util.ArrayList;
import java.util.Iterator;

public class IteratorDemo {
	public static void main(String[] args)  {	
		// ArrayList 선언 및 1 - 10까지 데이터를 입력합니다
        ArrayList<Integer> al = new ArrayList<Integer>();        
        for (int i = 0; i < 10; i++)
            al.add(i);
  
        // 결과 출력
        System.out.println(al);
  
        // Iterator 선언 및 ArrayList 데이터를 입력합니다
        Iterator itr = al.iterator();
  
        // 다음 데이터가 있는지 체크
        while (itr.hasNext())
        {
            // 다음 데이터 이동
            int i = (Integer)itr.next();
  
            // 결과 출력
            System.out.print(i + " ");
  
            // 홀 수 데이터 제거
            if (i % 2 != 0)
               itr.remove(); 
        }
        
        // 결과 출력 
        System.out.println(); 
        System.out.println(al);
	}
}
```

 

결과 화면입니다

Iterator를 사용하여 결과를 출력하고, remove를 사용하여 데이터를 제거할 수 있습니다



![img](https://blog.kakaocdn.net/dn/dNgcfK/btq5aNB7Noo/OcqFlaK9SKQlznLGlkOnfK/img.png)



 

------



## **ListIterator란?**

arrayList, linkedList 등과 같은 List 컬렉션에서 사용되는 클래스입니다

Iterator와 다르게 양방향을 지원합니다

데이터를 추가하거나 교체하는것도 가능합니다

 

ListIterator의 주요 메서드입니다

hasNext(), next(), remove()는 Iterator와 동일합니다

ListIterator에서만 사용이 가능한 메서드가 추가됩니다

| 메서드                | 설명                                                         |
| --------------------- | ------------------------------------------------------------ |
| boolean hasNext()     | 데이터가 계속해서 있다면 true를 반환, 없다면 false를 반환합니다 |
| Object next()         | 다음 데이터를 반환합니다 더 이상 데이터가 없으면 NoSuchElementException 에러가 발생합니다 |
| void remove()         | 다음 데이터를 제거합니다 이 메서드는 호출 당 한 번만 호출 할 수 있습니다 다음 메서드가 호출되지 않았거나 메서드의 마지막 호출 이후 사용하면 IllegalStateException 에러가 발생합니다 Collection이 제거 작업을 지원하지 않는 경우 UnsupportedOperationException 에러가 발생합니다 |
| boolean hasPrevious() | 이전 데이터가 있다면 true를 반환, 없다면 false를 반환합니다  |
| Object previous()     | 이전 데이터를 반환합니다                                     |
| void set(Object)      | 호출한 데이터를 Object로 교체합니다                          |
| void add(Object)      | 호출한 데이터의 앞에 Object를 추가합니다                     |

 

사용 예제를 통해서 위의 메서드에 대해서 알아보겠습니다

hasNext() next(), remove()는 Iterator와 사용방법이 똑같습니다

마찬가지로 hasPrevious(), previouse()도 반대로 읽어들이는 것만 다르지 hasNext(), next()와 사용법은 동일합니다

set(), add()를 사용하여 값을 추가할 수도 있습니다

```
import java.util.ArrayList;
import java.util.ListIterator;

public class listIteratorDemo {
	public static void main(String[] args)  {	
		// Array배열 선언 및 데이터 입력
        ArrayList al = new ArrayList();
        for (int i = 0; i < 10; i++)
            al.add(i);
  
        // 결과 출력
        System.out.println(al);
  
        //ListIterator 선언 및 데이터 입력
        ListIterator ltr = al.listIterator();
  
        // 다음 데이터가 있는지 체크
        while (ltr.hasNext())
        {
            //  다음 데이터로 이동
            int i = (Integer)ltr.next();
  
            // 결과 출력
            System.out.print(i + " ");
  
            // 짝수인 경우
            if (i%2==0)
            {
                i++;  // 값을 1 더함
                ltr.set(i);;  // 값을 교체 : 0 -> 1, 2 -> 3, 4 -> 5, 6 -> 7, 8 -> 9
                ltr.add(i);   // 호출한 데이터 앞에 값을 추가
            }
        }
        
        // 결과 출력
        System.out.println();
        System.out.println(al);
        
        // 다음 데이터가 있는지 체크
        while (ltr.hasPrevious())
        {
            //  다음 데이터로 이동
            int i = (Integer)ltr.previous();
  
            // 결과 출력
            System.out.print(i + " ");
        }
	}
}
```

 

결과 화면입니다

set(), add()를 사용하여 ListIterator에 데이터를 수정 및 추가했습니다

hasPrevious 및 previous를 사용하여 리스트를 역순으로 다시 조회합니다



![img](https://blog.kakaocdn.net/dn/rA79j/btq5bez0O50/UPcOcga5hIop39fq643EIk/img.png)



 



출처: https://crazykim2.tistory.com/559?category=686232 [차근차근 개발일기+일상:티스토리]