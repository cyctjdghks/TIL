# [Java] 자바 길이구하기
### 1. length란?
- 배열의 길이를 알고자 할때 사용된다.
```
int[] array01 = new int[12];
System.out.println(array01.length); //12
```

### 2. length()란?
- 문자열의 길이를 알고자 할때 사용된다.
```
String test = "test";
System.out.println(test.length()); //4
```

### 3. size()란?
- 컬렉션프레임워크(ArrayList, Set..등) 타입의 길이를 알고자 할때 사용된다.
```
ArrayList<Object> test = new ArrayList<Object>();
System.out.println(test.size()); //0
```