# java 21에 새로운 기능들

JDK 21은 15개의 신규 기능으로 구성된 신규 기능으로 구성되며, 이들은 총 4개의 카테고리로 구분할 수 있다.

- Core Java Library
    - JEP 431: [Sequenced Collections](https://openjdk.org/jeps/431)
    - JEP 442: [Foreign Function & Memory API (Third Preview)](https://openjdk.org/jeps/442)
    - JEP 444: [Virtual Threads](https://openjdk.org/jeps/444)
    - JEP 446: [Scoped Values (Preview)](https://openjdk.org/jeps/446)
    - JEP 448: [Vector API (Sixth Incubator)](https://openjdk.org/jeps/448)
    - JEP 453: [Structured Concurrency (Preview)](https://openjdk.org/jeps/453)
- Java Language Specification
    - JEP 430: [String Templates (Preview)](https://openjdk.org/jeps/430)
    - JEP 440: [Record Patterns](https://openjdk.org/jeps/440)
    - JEP 441: [Pattern Matching for switch](https://openjdk.org/jeps/441)
    - JEP 443: [Unnamed Patterns and Variables (Preview)](https://openjdk.org/jeps/443)
    - JEP 445: [Unnamed Classes and Instance Main Methods (Preview)](https://openjdk.org/jeps/445)
- HotSpot
    - JEP 439: [Generational ZGC](https://openjdk.org/jeps/439)
    - JEP 449: [Deprecate the Windows 32-bit x86 Port for Removal](https://openjdk.org/jeps/449)
    - JEP 451: [Prepare to Disallow the Dynamic Loading of Agents](https://openjdk.org/jeps/451)
- Security Library
    - JEP 452: [Key Encapsulation Mechanism API](https://openjdk.org/jeps/452)

## [Sequenced Collections](https://openjdk.org/jeps/431)

기존의 자바 컬렉션 프레임워크는 정해진 순서의 원소에 접근하는 것이 어려웠다. 예를 들어 List와 Deque는 모두 처음과 마지막 원소에 접근할 수 있지만, 이들의 공통 상위 인터페이스는 Collection라서 접근 방법이 다르다. SortedSet과 LinkedHashSet 역시 비슷한데, 이로 인해 다음과 같이 일관되지 못한 방법으로 원소에 접근하게 되는 것이다.

| | First element | Last element |
|----|---|---|
|List | list.get(0) | list.get(list.size() - 1) |
|Deque | deque.getFirst() | deque.getLast() |
|SortedSet | sortedSet.first() | sortedSet.last() |
|LinkedHashSet | linkedHashSet.iterator().next() | // missing |

Java 21부터는 정해진 순서의 원소를 조회할 수 있는 컬렉션을 표현하는 새로운 인터페이스를 도입한다. 이를 통해 정해진 순서의 원소(첫 원소와 마지막 원소 등)에 접근하고, 이를 역순으로 처리하기 위한 일관된 API를 제공한다.

![image](../img/Sequenced%20Collections.png)

### SequencedCollection

SequencedCollection은 처음과 마지막 요소에 대한 공통된 기능을 제공한다. reversed()는 새롭게 추가된 메소드이지만, 나머지는 Deque로부터 승격된 것이다.

```java
interface SequencedCollection<E> extends Collection<E> {
    // new method
    SequencedCollection<E> reversed();
    // methods promoted from Deque
    void addFirst(E);
    void addLast(E);
    E getFirst();
    E getLast();
    E removeFirst();
    E removeLast();
}
```

추가로 제공되는 reversed()는 기존 컬렉션에서 역순으로 원소들을 처리할 수 있게 된다. 예를 들어 iterator()나 forEach(), stream(), parallelStream(), toArray() 등과 같은 기능을 역순 상태의 원소로 호출 가능해지는 것이다.

```java
linkedHashSet.reversed().stream()
```

### SequencedSet

SequencedSet은 중복된 원소를 갖지 않는 SequencedCollection에 해당하는 Set이다. SortedSet과 같은 컬렉션은 SequencedCollection 부모 인터페이스에 선언된 addFirst(E) 및 addLast(E) 메소드 등을 지원할 수 없기 때문에 UnsupportedOperationException을 던질 수 있다.

```java
interface SequencedSet<E> extends Set<E>, SequencedCollection<E> {
    SequencedSet<E> reversed();    // covariant override
}
```

addFirst(E)와 addLast(E)는 LinkedHashSet과 같은 구현체에 특별한 의미를 갖는다. 왜냐하면 원소가 이미 존재하는 경우에 적절한 위치로 재배치되기 때문이다. 이는 요소를 재배치할 수 없다는 LinkedHashSet의 오랜 결함을 해결해준다.

### SequencedMap

SequencedMap 역시 정해진 순서의 원소에 대한 공통 기능을 제공한다. SequenceSet에서와 마찬가지로 putFirst(K, V)와 putLast(K, V) 메소드는 원소가 이미 존재하는 경우에 적절한 위치로 재배치되는 특별한 의미를 갖는다. 이를 처리할 수 없는 SortedMap은 마찬가지로 UnsupportedOperationException을 던질 수 있다.

```java
interface SequencedMap<K,V> extends Map<K,V> {
    // new methods
    SequencedMap<K,V> reversed();
    SequencedSet<K> sequencedKeySet();
    SequencedCollection<V> sequencedValues();
    SequencedSet<Entry<K,V>> sequencedEntrySet();
    V putFirst(K, V);
    V putLast(K, V);
    // methods promoted from NavigableMap
    Entry<K, V> firstEntry();
    Entry<K, V> lastEntry();
    Entry<K, V> pollFirstEntry();
    Entry<K, V> pollLastEntry();
}
```

### Collections

새로운 인터페이스가 추가됨에 따라 Collections 유틸에도 다음과 같은 기능이 추가될 예정이다.

```java
Collections.unmodifiableSequencedCollection(sequencedCollection)
Collections.unmodifiableSequencedSet(sequencedSet)
Collections.unmodifiableSequencedMap(sequencedMap)
```


## [Virtual Threads](https://openjdk.org/jeps/444)

가상 쓰레드는 처리량이 많은 동시성 애플리케이션을 개발하고 모니터링하고 유지 및 관리하는데 드는 비용을 획기적으로 줄여줄 경량 쓰레드이다. 자바 진영(스프링 MVC)에서는 멀티쓰레드 모델을 기본적인 동시성 처리 방법으로 사용해왔고, 이로 인해 I/O 요청이 들어오면 쓰레드가 블로킹되면서 자원이 낭비되곤 했다. 코틀린과 같은 언어에서 코루틴을 이용해 이를 해결할 수도 있지만, 이를 위해 특정 키워드(async-await 또는 suspend 등과 같은)를 통해 논블로킹 구간을 지정해주어야 한다.

```java
fun main() = runBlocking {
    doWorld()
}

suspend fun doWorld() = coroutineScope {  // this: CoroutineScope
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
}
```

하지만 자바는 이를 JVM 레벨에서 처리하도록 하였고, 따라서 개발자는 키워드를 명시해주지 않아도 JVM이 알아서 논블로킹 처리를 해주기 때문에 많은 이점을 가질 수 있다.

## [Record Patterns](https://openjdk.org/jeps/440)

레코드(Record)에 타입 패턴을 함께 적용하여 레코드의 값을 손쉽게 처리할 수 있도록 도와준다.

자바 16에서는 instance of 연산자에 타입 패턴을 적용하여 패턴 매칭을 개선시켰다. 타입 패턴 덕분에 instance of 하위 블록에서는 직접적으로 타입 캐스팅할 필요가 없어졌다.

```java
// Prior to Java 16
if (obj instanceof String) {
    String s = (String)obj;
    ... use s ...
}

// As of Java 16
if (obj instanceof String s) {
    ... use s ...
}
```

이러한 바탕은 자바 언어가 보다 데이터 중심적인 프로그래밍 스타일로 나아가기 위함인데, 자바 21부터는 레코드 타입에 대해 보다 개선된 방법으로 사용 가능해질 예정이다.

기존에는 레코드 클래스에 instance of 연산자를 적용하면 다음과 같이 사용했었다.

```java
record Point(int x, int y) {}

static void printSum(Object obj) {
    if (obj instanceof Point p) {
        int x = p.x();
        int y = p.y();
        System.out.println(x+y);
    }
}
```

하지만 자바 21부터는 다음과 같은 방식으로 사용 가능해진다. 이를 통해 obj가 Point의 인스턴스인지 여부를 테스트할 뿐만 아니라 개발자를 대신해 접근자 메소드를 호출하여 직접 변수들에 접근할 수 있게 된다. 이렇듯 새롭게 도입되는 레코드 패턴(Record Pattern)은 레코드 객체를 분해해주는 기능이라고 볼 수 있다.

```java
// As of Java 21
static void printSum(Object obj) {
    if (obj instanceof Point(int x, int y)) {
        System.out.println(x+y);
    }
}
```

## [Pattern Matching for switch](https://openjdk.org/jeps/441)

### Instanceof 사용

기존의 스위치 문은 특정 타입 여부를 검사하는 것이 상당히 제한적이였기 때문에, 특정 타입인지 검사하려면 instance of에 if-else 문법을 사용해야 했다.

```java
// Prior to Java 21
static String formatter(Object obj) {
    String formatted = "unknown";
    if (obj instanceof Integer i) {
        formatted = String.format("int %d", i);
    } else if (obj instanceof Long l) {
        formatted = String.format("long %d", l);
    } else if (obj instanceof Double d) {
        formatted = String.format("double %f", d);
    } else if (obj instanceof String s) {
        formatted = String.format("String %s", s);
    }
    return formatted;
}
```

하지만 자바 21부터는 이러한 부분을 개선하였고, 다음과 같은 간결한 방식으로 패턴 매칭을 처리할 수 있게 되었다.

```java
// As of Java 21
static String formatterPatternSwitch(Object obj) {
    return switch (obj) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> obj.toString();
    };
}
```

### null 검사

기존에는 스위치 문의 파라미터가 null이라면 NPE(NullPointerException)을 던지기 때문에, null에 대한 검사가 스위치 문 외부에서 수행되어야 했다.

```java
// Prior to Java 21
static void testFooBarOld(String s) {
    if (s == null) {
        System.out.println("Oops!");
        return;
    }
    switch (s) {
        case "Foo", "Bar" -> System.out.println("Great");
        default           -> System.out.println("Ok");
    }
}
```
```
하지만 자바21부터는 이러한 부분을 개선하였고, null에 해당하는 케이스를 스위치 내부에서 검사할 수 있게 되었다.

```java
// As of Java 21
static void testFooBarNew(String s) {
    switch (s) {
        case null         -> System.out.println("Oops");
        case "Foo", "Bar" -> System.out.println("Great");
        default           -> System.out.println("Ok");
    }
}
```

### case 세분화

case 문은 여러 값에 대한 검사를 필요로 하기 때문에 세분화될 수 있다. 자바 21 이전에는 이러한 부분을 코드로 작성하면 상당히 복잡한 구조로 만들 수 밖에 없었다.

```java
// As of Java 21
static void testStringOld(String response) {
    switch (response) {
        case null -> { }
        case String s -> {
            if (s.equalsIgnoreCase("YES"))
                System.out.println("You got it");
            else if (s.equalsIgnoreCase("NO"))
                System.out.println("Shame");
            else
                System.out.println("Sorry?");
        }
    }
}
```

자바 21부터는 이러한 부분 역시 개선이 되었고, 다음과 같이 세분화할 수 있게 되었다. 이를 통해 보다 읽기 좋은 스위치 문 작성이 가능해진 것이다.

```java
// As of Java 21
static void testStringNew(String response) {
    switch (response) {
        case null -> { }
        case String s
        when s.equalsIgnoreCase("YES") -> {
            System.out.println("You got it");
        }
        case String s
        when s.equalsIgnoreCase("NO") -> {
            System.out.println("Shame");
        }
        case String s -> {
            System.out.println("Sorry?");
        }
    }
}
```

### enum 개선

기존에는 스위치 문에서 enum을 사용하는 것이 상당히 제한적이였다.

```java
// Prior to Java 21
public enum Suit { CLUBS, DIAMONDS, HEARTS, SPADES }

static void testforHearts(Suit s) {
    switch (s) {
        case HEARTS -> System.out.println("It's a heart!");
        default -> System.out.println("Some other suit");
    }
}
```

앞서 살펴본 새로운 문법인 case 세분화를 도입하여도, 코드는 불필요하게 장황해졌고, 복잡성이 완전히 해결되지 않았다.

```java
// As of Java 21
sealed interface CardClassification permits Suit, Tarot {}
public enum Suit implements CardClassification { CLUBS, DIAMONDS, HEARTS, SPADES }
final class Tarot implements CardClassification {}

static void exhaustiveSwitchWithoutEnumSupport(CardClassification c) {
    switch (c) {
        case Suit s when s == Suit.CLUBS -> {
            System.out.println("It's clubs");
        }
        case Suit s when s == Suit.DIAMONDS -> {
            System.out.println("It's diamonds");
        }
        case Suit s when s == Suit.HEARTS -> {
            System.out.println("It's hearts");
        }
        case Suit s -> {
            System.out.println("It's spades");
        }
        case Tarot t -> {
            System.out.println("It's a tarot");
        }
    }
}
```

그래서 이러한 부분을 완전히 해결하고자 다음과 같은 문법으로 코드를 작성할 수 있도록 개선되었다. 이를 통해 더욱 가독성있는 enum 관련 스위치 문을 작성할 수 있을 것이다.

```java
// As of Java 21
static void exhaustiveSwitchWithBetterEnumSupport(CardClassification c) {
    switch (c) {
        case Suit.CLUBS -> {
            System.out.println("It's clubs");
        }
        case Suit.DIAMONDS -> {
            System.out.println("It's diamonds");
        }
        case Suit.HEARTS -> {
            System.out.println("It's hearts");
        }
        case Suit.SPADES -> {
            System.out.println("It's spades");
        }
        case Tarot t -> {
            System.out.println("It's a tarot");
        }
    }
}
```

## [Generational ZGC](https://openjdk.org/jeps/439)

ZGC는 짧은 지연 시간과 높은 확정성을 위해 고안된 GC 알고리즘으로 Java 15부터 프로덕션 환경에서 사용할 수 있게 되었다. 약한 세대 가설(Weak Generational Hypothesis)을 따라 대부분의 객체는 금방 죽기 때문에, 금방 죽는 Young 영역과 오래 살아남는 Old 영역을 분리하여 관리하는 것이 좋다. 그래서 Java 21에서는 이러한 방식을 통해 ZGC의 기능을 확장하여 성능을 개선시키고자 하였다.

## [Deprecate the Windows 32-bit x86 Port for Removal](https://openjdk.org/jeps/449)

이후 릴리스에서 Windows 32bit x86 port를 제거하기 위해, 이를 Deprecate 시켰다. 이제 Windows 32bit 용 빌드를 구성하려고 시도할 때 오류 메세지가 표시된다.

```
$ bash ./configure
...
checking compilation type... native
configure: error: The Windows 32-bit x86 port is deprecated and may be removed in a future release. \\
Use --enable-deprecated-ports=yes to suppress this error.
configure exiting with result code 1
$
```

## [Prepare to Disallow the Dynamic Loading of Agents](https://openjdk.org/jeps/451)
자바는 agent를 통해 애플리케이션 코드를 동적으로 변경하도록 지원해왔고, 이를 통해 애플리케이션을 모니터링하고 관찰하는 많은 방법들이 탄생하게 되었다. 대표적으로 Pinpoint와 같은 도구는 자바 에이전트를 기반으로 바이트 코드를 조작하여 모니터링을 돕는 APM 도구이다.

이러한 자바 애플리케이션을 프로파일링하는 정상적인 방법들은 애플리케이션이 실행될 때 불러와지고, 애플리케이션이 실행되는 중간에 불러와지는 경우가 거의 없다. 따라서 자바 21에서는 실행 중인 JVM에 에이전트가 동적으로 로드될 때 경고를 발행시키도록 수정되었다. 물론 JVM 시작 시에 에이전트를 로드하는 것은 경고를 발생시키지 않는다.

## [Key Encapsulation Mechanism API](https://openjdk.org/jeps/452)
Java21에는 공개 키 암호화를 사용하여 대칭 키를 보호하는 암호화 기술인 KEM(Key Encapsulation Mechanism) API가 도입되었다. 기존의 기술은 무작위로 생성된 대칭 키를 공개 키로 암호화하는 것이지만, 패딩이 필요하고 보안을 증명하기 어려울 수 있다. 대신 KEM은 공개 키의 속성을 사용하여 패딩이 필요 없는 관련 대칭 키를 도출한다.

KEM은 양자 공격을 방어하기 위한 핵심 도구가 될 것이다. 다른 보안 제공업체들은 이미 표준 KEM API에 대한 필요성을 표명했고, 자바 역시 이를 공식적으로 도입하기로 결정하였다.

```java
package javax.crypto;

public class DecapsulateException extends GeneralSecurityException;

public final class KEM {

    public static KEM getInstance(String alg)
        throws NoSuchAlgorithmException;
    public static KEM getInstance(String alg, Provider p)
        throws NoSuchAlgorithmException;
    public static KEM getInstance(String alg, String p)
        throws NoSuchAlgorithmException, NoSuchProviderException;

    public static final class Encapsulated {
        public Encapsulated(SecretKey key, byte[] encapsulation, byte[] params);
        public SecretKey key();
        public byte[] encapsulation();
        public byte[] params();
    }

    public static final class Encapsulator {
        String providerName();
        int secretSize();           // Size of the shared secret
        int encapsulationSize();    // Size of the key encapsulation message
        Encapsulated encapsulate();
        Encapsulated encapsulate(int from, int to, String algorithm);
    }

    public Encapsulator newEncapsulator(PublicKey pk)
            throws InvalidKeyException;
    public Encapsulator newEncapsulator(PublicKey pk, SecureRandom sr)
            throws InvalidKeyException;
    public Encapsulator newEncapsulator(PublicKey pk, AlgorithmParameterSpec spec,
                                        SecureRandom sr)
            throws InvalidAlgorithmParameterException, InvalidKeyException;

    public static final class Decapsulator {
        String providerName();
        int secretSize();           // Size of the shared secret
        int encapsulationSize();    // Size of the key encapsulation message
        SecretKey decapsulate(byte[] encapsulation) throws DecapsulateException;
        SecretKey decapsulate(byte[] encapsulation, int from, int to,
                              String algorithm)
                throws DecapsulateException;
    }

    public Decapsulator newDecapsulator(PrivateKey sk)
            throws InvalidKeyException;
    public Decapsulator newDecapsulator(PrivateKey sk, AlgorithmParameterSpec spec)
            throws InvalidAlgorithmParameterException, InvalidKeyException;

}
```

#### 참고자료
- https://openjdk.org/projects/jdk/21/
- https://mangkyu.tistory.com/308