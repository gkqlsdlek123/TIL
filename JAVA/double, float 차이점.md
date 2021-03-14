double와 float의 차이점에 대해서 알아보겠습니다

 

------

## **double과 float 차이점**

밑에서 볼 수 있듯이 float는 4바이트의 수까지 표현하고, double은 8바이트까지 수를 표현합니다

double이 좀 더 큰 숫자까지 표현을 할 수 있습니다



![img](https://blog.kakaocdn.net/dn/EdvmZ/btq4dcQq08n/8gSdsr9UipzcEuliOSLDDk/img.png)



 

아래에서 볼 수 있듯이 float는 소수점 7자리까지 표현을 해주고

double은 소수점 16자리까지 표현을 해줍니다



![img](https://blog.kakaocdn.net/dn/23ons/btq4ct6dyfI/K9ePQ7T6UZuGLSL9JqwShK/img.png)



 

나누기를 할 경우 소수점 float는 소수점 7자리까지 double은 소수점 16자리까지 표현을 합니다

예제 코드를 확인해보겠습니다

------

## **예제 코드**

```
public class FloatDouble {

	public static void main(String[] args) {
		double d = 10.0 / 3.0;
		float f = 10.0f / 3.0f;
		
		System.out.println("Double : " + d);
		System.out.println("Float : " + f);
	}
}
```

 

결과 화면



![img](https://blog.kakaocdn.net/dn/bW3M3i/btq4f9S1lZ3/Lo6sntdCnEx5wuGsFUWXZ1/img.png)

