this와 super의 사용방법에 대해서 알아보겠습니다

 

------

 

## **this()**

**this()란?**

현재 클래스 안의 개체를 가져오는 참조 변수를 말합니다

 

**사용 예제**

부모, 자식 폼에서 부모 폼의 같은 변수가 있어도 this를 사용하여 자식 변수의 데이터를 가져옵니다

```
public class ThisSuper {
	public static void main(String[] args) {
		// 자식 호출
		Child child = new Child();

		
		// 자식에서 메서드 호출
		child.CrazyKim();
	}
}

// 부모 클래스 선언 
class Mother {
	public String blog = "MoCrazyKim";	
	public int Period = 6;

	public void CrazyKim() {
		System.out.println(blog + "의 블로그입니다. 블로그는 만들어진지 " + Period + "년이 됐습니다.");
	}
}

// 자식 클래스 선언
class Child extends Mother {
	String blog = "ChCrazyKim";	
	int Period = 10;	

	public void CrazyKim() {
		System.out.println(this.blog + "의 블로그입니다. 블로그는 만들어진지 " + this.Period + "년이 됐습니다.");
	}
}
```

 

**결과 화면**



![img](https://blog.kakaocdn.net/dn/cfnjdk/btq49RxbMxc/uy6fICGR16YcbtodmKDAUk/img.png)



 

------

 

## **super()**

**super()란?**

자식 클래스에서 부모 클래스 개체를 가져오는데 사용하는 참조 변수입니다

 

**사용 예제**

super를 사용하여 자식 클래스에서 메서드 및 변수를 호출하는 예제를 알아보겠습니다

아래의 방법대로 super.CrazyKim(); 을 사용하여 부모 클래스에 있는 CrazyKim 메서드를 호출합니다

세 번째의 super.blog와 super.Period로 부모클래스에 있는 blog와 Period의 값을 호출하게 됩니다

```
public class ThisSuper {
	public static void main(String[] args) {
		// 자식 호출
		Child child = new Child();
		
		// 자식에서 메서드 호출
		child.CrazyKim();
	}
}

// 부모 클래스 선언 
class Mother {
	public String blog = "MoCrazyKim";	
	public int Period = 6;

	public void CrazyKim() {
		System.out.println(blog + "의 블로그입니다. 블로그는 만들어진지 " + Period + "년이 됐습니다.");
	}
}

// 자식 클래스 선언
class Child extends Mother {
	String blog = "ChCrazyKim";	
	int Period = 10;	

	public void CrazyKim() {
		super.CrazyKim();
		System.out.println(blog + "의 블로그입니다. 블로그는 만들어진지 " + Period + "년이 됐습니다.");
		System.out.println(super.blog + "의 블로그입니다. 블로그는 만들어진지 " + super.Period + "년이 됐습니다.");
	}
}
```

 

**결과 화면**



![img](https://blog.kakaocdn.net/dn/ckMEbe/btq4YslWe4A/IeoJ9Fd8Kcp1uzMkhngndk/img.png)



 

여기까지 자바의 this와 super의 사용법에 대해서 알아봤습니다