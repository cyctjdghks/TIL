# JAVA ThreadLocal이란

## ThreadLocal이란,

- Thread 내부에서 사용하는 지역변수입니다.

일반 변수의 수명은 특정 코드 블록 범위 내에서만 유효하지만, ThreadLocal을 이용하면 쓰레드 영역에 변수를 설정할 수 있기 때문에, 특정 쓰레드가 실행하는 모든 코드에서 그 쓰레드에 설정된 변수 값을 사용할 수 있게 됩니다.

멀티 쓰레드 환경에서 각 쓰레드마다 get(), set() 메서드를 통해 독립적으로 변수에 접근할 수 있습니다.

## ThreadLocal의 주요 특징

1. 스레드별로 데이터 저장: ThreadLocal 변수는 각 스레드에게 별도로 할당되며, 한 스레드에서 설정한 값은 다른 스레드에게 직접적으로 노출되지 않습니다.

2. 간단한 API: ThreadLocal은 간단한 API를 제공하여 데이터를 설정하고 가져오는 데 사용됩니다.

3. 초기값 설정: ThreadLocal 변수는 초기값을 설정할 수 있으며, 각 스레드에서 처음으로 해당 변수에 접근할 때 이 초기값이 사용됩니다.

```java
public class MyThreadLocalExample {
    // ThreadLocal 변수를 정의하고 초기값을 설정합니다.
    private static ThreadLocal<Integer> threadLocalValue = ThreadLocal.withInitial(() -> 0);

    public static void main(String[] args) {
        // 각 스레드에서 ThreadLocal 변수를 사용
        Thread thread1 = new Thread(() -> {
            threadLocalValue.set(42);
            int value = threadLocalValue.get();
            System.out.println("Thread 1: " + value); // 출력: Thread 1: 42
        });

        Thread thread2 = new Thread(() -> {
            int value = threadLocalValue.get();
            System.out.println("Thread 2: " + value); // 출력: Thread 2: 0
        });

        thread1.start();
        thread2.start();
    }
}
```

위의 예제에서는 두 개의 스레드가 ThreadLocal 변수인 threadLocalValue에 접근하며, 각 스레드에서 저장한 값이 서로 독립적으로 유지됩니다.

ThreadLocal은 스레드 간 데이터 공유를 효과적으로 제어할 수 있지만, 오용할 경우 메모리 누수와 같은 문제가 발생할 수 있으므로 주의해서 사용해야 합니다.


## ThreadLocal의 활용은 다음과 같습니다.

ThreadLocal은 한 쓰레드에서 실행되는 코드가 동일한 객체를 사용할 수 있도록 해 줍니다.
따라서, 쓰레드와 관련된 코드에서 변수를 공유할 때, 파라미터 또는 리턴 값으로 정보를 제공해주지 않아도 됩니다.

대표적인 활용 예시는 Spring Security에서 사용자 인증 정보를 사용입니다.
서버에서 클라이언트 요청들에 대해 각 쓰레드에서 처리하게 될 경우, 해당 유저의 인증 및 세션 정보나 참조 데이터를 저장하는데 ThreadLocal이 사용됩니다.
 

## ThreadLocal의 내부 구조

ThreadLocal의 내부는 thread 정보를 key로 하여 값을 저장해두는 Map 구조를 가지고 있습니다.

기본적인 사용에는 get, set 메서드를 이용합니다.


## 쓰레드 로컬 사용 시 주의 사항

ThreadLocal은 Thead의 정보를 key로 하여 Map의 형식으로 데이터를 저장한 후 사용할 수 있는 자료구조를 가지고 있습니다.

따라서 만약 ThreadPool을 사용하여 thread를 재활용한다면
동일한 이전에 세팅했던 ThreadLocal의 정보가 남아있어 원치않는 동작을 할 수 있습니다. 
따라서 ThreadPool을 사용하는 경우에는 반드시 모두 사용 후 THreadLocal의 값을 remove 메서드를 사용하여 값을 제거해주는것이 필요합니다.
