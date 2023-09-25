# JVM 구조

JVM의 구조는 Class Loader, Exection engine, Runtime Data Area, Garbage Collector로 이루어져 있습니다.

![image](../img/JVM%20구조.png)

- `Class Loader`는 자바 컴파일러가 .java 파일을 컴파일하여 .class 파일(바이트 코드)이 생성되면,
이 생성된 클래스 파일들을 엮어 Runtime Data Area 형태로 메모리에 적재하는 역할을 합니다.
- `Execution Engine`는 클래스 로더를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명령어단위로 읽어서 실행합니다. 메모리에 적재된 클래스들을 기계어로 변경해 명령어 단위로 실행하는 역할을 합니다.
- `Garbage Collector`는 Heap 메모리 영역에 생성 된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 합니다.
Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로 부터 별도로 할당 받은 메모리 공간을 말합니다.
- `Runtime Data Area`는 크게 5가지 영역으로 나눌 수 있습니다.
    - 모든 스레드에서 공유하는 영역
        - Method Area
            - 메소드 영역에서 자바 프로그램의 클래스 코드, 변수 코드, static, final 변수 등이 생성된다.
        - Heap Area
            - new 키워드로 생성한 객체가 저장되는 영역
            - 동적으로 생성된 객체와 배열이 저장되는 곳으로 Garbage Collection의 대상이 되는 영역이다.
        - Stack Area
            - 지역 변수, 파라미터 등이 생성되는 영역, 동적으로 객체를 생성하면 실제 객체는 Heap에 할당되고 해당 레퍼런스만 Stack에 저장된다.
            - Stack은 스레드별로 독자적으로 가진다.
            - Heap에 있는 객체가 Stack에서 참조 할 수 없는 경우 GC의 대상이 된다.
    - 각 스레드 별로 생성되는 영역
        - PC Register
            - 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있다.(CPU의 PC Register와 다르다.)
        - Native Method
            - 자바외 언어로 작성된 네이티브 코드를 위한 메모리 영역