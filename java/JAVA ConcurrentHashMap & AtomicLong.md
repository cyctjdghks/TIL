# JAVA ConcurrentHashMap & AtomicLong

## ConcurrentHashMap이란?

- Java에서 멀티스레드 환경에서도 안전하게 사용할 수 있는 Map 구현체
- HashMap과 달리 동기화 처리가 되어 있어 여러 스레드가 동시에 접근해도 안전함

---

## 주요 특징 (ConcurrentHashMap)

1. **동기화 처리**

   - 내부적으로 Segment 또는 Bucket 단위로 락을 걸어 동시성 처리
   - Java 8부터는 `synchronized` 대신 `CAS + bucket 구조` 기반으로 개선됨

2. **null 허용 X**

   - 키와 값 모두 null 저장 불가

3. **Fail-safe Iterator**

   - 순회 중 구조 변경이 가능
   - ConcurrentModificationException 발생하지 않음

4. **성능**
   - Hashtable보다 락 범위가 작아 성능이 우수함

---

## 사용 예제 (ConcurrentHashMap)

```java
import java.util.concurrent.ConcurrentHashMap;

public class Example {
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();

        map.put("apple", 1);
        map.put("banana", 2);

        map.computeIfAbsent("orange", k -> 3); // 값 없으면 추가

        System.out.println(map.get("orange")); // 출력: 3
    }
}
```

---

## AtomicLong이란?

- 멀티스레드 환경에서 long 값을 원자적으로 다룰 수 있는 클래스
- 락 없이도 안전하게 값을 증가시키거나 읽고 쓸 수 있음
- 내부적으로 **CAS(compare-and-swap)** 연산을 사용  
  → 예상한 값과 실제 값이 같을 때만 값을 갱신하는 방식

---

## 주요 특징 (AtomicLong)

1. **원자성 보장**

   - `incrementAndGet()`, `getAndIncrement()`, `addAndGet()` 등의 메서드 제공

2. **lock-free 구조**

   - synchronized 없이도 높은 성능을 유지

3. **CAS 기반 동작**
   - compare-and-set으로 예상 값과 실제 값을 비교해 조건부 갱신 수행

---

## 사용 예제 (AtomicLong)

```java
import java.util.concurrent.atomic.AtomicLong;

public class AtomicLongExample {
    public static void main(String[] args) {
        AtomicLong counter = new AtomicLong(0);

        long val1 = counter.incrementAndGet();  // 1
        long val2 = counter.addAndGet(5);       // 6
        boolean updated = counter.compareAndSet(6, 10); // true

        System.out.println(counter.get()); // 출력: 10
    }
}
```

---

## 요약 비교

| 항목             | ConcurrentHashMap              | AtomicLong                 |
| ---------------- | ------------------------------ | -------------------------- |
| 목적             | 스레드 안전한 Map 구현체       | 스레드 안전한 long 값 연산 |
| 동시성 처리 방식 | 버킷 기반 락, 세분화된 락 처리 | CAS 기반 lock-free 처리    |
| null 허용 여부   | ❌ 허용 안 함                  | N/A                        |
| 대표 메서드      | put, get, computeIfAbsent      | incrementAndGet, addAndGet |
| 사용 예시        | 공유 맵, 캐시, 세션 저장 등    | 요청 카운터, 통계 수집 등  |

→ **ConcurrentHashMap은 공유 데이터를 안전하게 보관하고, AtomicLong은 안전하게 숫자를 조작한다.**

---

## CAS(Compare-And-Swap)란?

**CAS**는 멀티스레드 환경에서 동기화 없이 공유 데이터를 **원자적으로 업데이트**할 수 있게 해주는 비동기적 알고리즘(Non-blocking Algorithm)이다.

### 🔹 작동 원리

1. **예상 값(expected value)** 과
2. **현재 메모리 값(current value)** 을 비교해서
3. 값이 일치하면 새로운 값으로 **원자적 교체**

이 연산은 보통 CPU 수준에서 제공되는 명령어(예: `cmpxchg`)를 통해 수행됨.

```java
boolean compareAndSet(expectedValue, newValue);
```

- `expectedValue`가 현재 값과 같을 때만 `newValue`로 교체
- 다르면 아무 일도 하지 않고 `false` 반환

### 🔹 장점

- **lock-free**로 동작 → 데드락 위험 없음
- 경쟁이 심하지 않을 경우 synchronized보다 훨씬 빠름

### 🔹 단점

- **ABA 문제**: 값이 A→B→A로 변경되면 CAS가 감지하지 못함
  - 이 문제를 방지하기 위해 `AtomicStampedReference`, `VarHandle`, `Versioning` 등을 함께 사용하기도 함

---

> CAS는 Java의 `AtomicXXX` 클래스와 `ConcurrentHashMap`의 내부 연산에도 핵심적으로 사용되는 알고리즘이다.

---

## 참고

- [Java 공식 문서 - ConcurrentHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html)
- [Java 공식 문서 - AtomicLong](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/atomic/AtomicLong.html)
- [[JAVA] HashMap,HashTable,ConcurrentHashMap 비교](https://velog.io/@ch0jm/JAVA-HashMap)
- [ConcurrentHashmap 동시성 처리 방법](https://thewayitwas.tistory.com/608)
- [[Java] AtomicLong 사용 방법](https://roeldowney.tistory.com/536)
