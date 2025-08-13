## Java 가변 인수 (Variable Arguments)

### 1. 개념

**역할**

- 메서드가 **인수 개수에 상관없이** 동적으로 매개변수를 받을 수 있도록 해주는 문법입니다.
- 내부적으로는 전달된 인자들을 **배열로 받아** 처리합니다.
- JDK 1.5부터 지원되며, 대표적으로 `System.out.printf()` 메서드가 가변 인수를 사용합니다.

**문제 상황**

- 매개변수 개수가 유동적인 메서드를 구현할 때, 기존에는 **메서드 오버로딩**으로 해결했지만, 이는 비효율적입니다.
- 가변 인수를 사용하면 매개변수 개수에 따라 메서드를 여러 개 만들 필요 없이 **한 번에 처리**할 수 있습니다.

**예시**

```java
print("홍길동");
print("홍길동", "이순신");
print("홍길동", "이순신", "유성룡");
print("홍길동", "이순신", "유성룡", "강감찬");
print("홍길동", "이순신", "유성룡", "강감찬", "이도");
```

---

### 2. 사용법

**선언 방법**

- 메서드 파라미터에 `타입... 변수명` 형태로 선언합니다.
- 전달 인자를 0개부터 n개까지 받을 수 있습니다.
- **반드시** 가변 인수는 **메서드 매개변수 목록의 마지막**에 위치해야 합니다.

**예시**

```java
public static void main(String[] args) {
    print("홍길동");
    print("홍길동", "이순신");
    print("홍길동", "이순신", "유성룡");
    print("홍길동", "이순신", "유성룡", "강감찬");
    print("홍길동", "이순신", "유성룡", "강감찬", "이도");
}

public static void print(String... str) {
    // str은 String[] 타입으로 받아들인다.
    for (String s : str) {
        System.out.print(s + ", ");
    }
    System.out.println();
}
```

---

### 3. 다른 매개변수와 함께 사용 시 규칙

- 가변 인수가 다른 매개변수와 함께 쓰일 경우 **반드시 마지막**에 위치해야 합니다.
- 전달 순서: **고정 매개변수 → 나머지 가변 인수**

**예시**

```java
public static void main(String[] args) {
    print(1, true, "홍길동", "이순신", "유성룡");
}

public static void print(int num, boolean bool, String... str) {
    System.out.println("number : " + num);
    System.out.println("bool : " + bool);
    System.out.println("rest parameters : " + Arrays.toString(str));
}
```

---

### 4. 자바스크립트의 가변 인수

- 자바스크립트에서도 가변 인수가 존재하며 **나머지 매개변수(rest parameter)** 또는 **spread 연산자**로 불립니다.

```javascript
function print(...data) {
  console.log(data);
}
print(1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

---

### 5. 주의 사항

#### 5-1. 성능 문제

- 호출 시마다 배열이 생성되므로 성능 민감 구간에선 비효율적일 수 있습니다.
- 자주 쓰는 인자 개수에 맞춰 **오버로딩 + 가변 인수 혼합**을 고려할 수 있습니다.

```java
class Printer {
    public void print(int a1) {}
    public void print(int a1, int a2) {}
    public void print(int a1, int a2, int a3) {}
    public void print(int a1, int a2, int a3, int... rest) {}
}
```

#### 5-2. 오버로딩 금지

- 같은 클래스에서 가변 인수를 사용하는 메서드를 오버로딩하면 **컴파일러가 모호성**으로 에러를 발생시킬 수 있습니다.

```java
class Printer {
    public void print(String c, String... str) {
        System.out.println("첫번째 메서드");
    }
    public void print(String... str) {
        System.out.println("두번째 메서드");
    }
}

Printer p = new Printer();
p.print("-", "1", "2", "3"); // 컴파일 에러 발생 가능
```

#### 5-3. 배열 타입 매개변수와 혼용 불가

- 내부적으로 배열을 사용하므로, 동일 시그니처로 배열 매개변수와 가변 인수를 함께 정의할 수 없습니다.

#### 5-4. 제네릭과 함께 사용할 때 주의

- 제네릭 타입은 런타임에 타입 정보가 소거되므로, 가변 인수 배열을 외부로 노출하면 **ClassCastException**이 발생할 수 있습니다.

```java
class Printer {
    public <T> T[] toArray(T... args) {
        return args; // Object[]로 반환될 수 있음
    }
    public <T> T[] pick(T a, T b, T c) {
        return toArray(a, b, c);
    }
}

Printer p = new Printer();
String[] s = p.pick("1", "2", "3"); // 런타임 캐스팅 오류 발생 가능
```

- 해결 방법:
  - 배열 대신 `List<T>`로 타입을 제한
  - 또는 가변 인수 배열을 외부에 노출하지 않도록 설계

---

**출처:** [Inpa Dev - 자바 가변 인수(Varargs)](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B3%80-%EC%9D%B8%EC%88%98Varargs-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EB%A5%BC-%EB%8F%99%EC%A0%81%EC%9C%BC%EB%A1%9C)
