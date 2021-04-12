```
HashMap과 HashTable의 차이점에 대해서 알아보겠습니다
 

 
Map 인터페이스
HashMap과 HashTable은 Map인터페이스에서 상속을 받아 구현이 되어집니다
둘 다 Key와 Value로 구분되어 값을 관리합니다
데이터를 찾을 때 Key를 기준으로 검색하여 Value를 가져옵니다
값을 탐색함에 있어서 높은 효율을 기다할 수 있습니다
 

 
차이점
본격적으로 두 클래스의 차이점입니다
 
1. 동기화(Synchronization)
HashMap은 동기화를 제공하고, HashTable은 동기화를 제공합니다
멀티스레드 환경에서는 HashTable을 사용하는 것이 유리합니다
하지만 멀티스레드 환경이 아닌 부분에서는 HashTable이 HashMap보다 성능이 떨어진다는 단점이 있습니다
 
2. Null 여부
HashMap에서는 null을 허용하고 HashTable에서는 null을 허용하지 않습니다
 
3. Enumeration(열거) 여부
HashTable은 Not Fail-Fast Enumeration을 제공합니다
HashMap은 Enumeration을 제공하지 않습니다
 
4. 보조해시함수
보조해시함수의 역할은 Key의 해시값을 변경해 해시 충돌 가능성을 줄여주는 역할을 합니다
HashMap은 보조해시를 사용하기 때문에 HashTable에 비해서 해시 충돌(Hash Collision)이 덜 발생하여 성능적인 이점이 있습니다
```

