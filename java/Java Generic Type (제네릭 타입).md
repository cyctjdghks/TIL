# Java Generic Type (제네릭 타입)

Java에서 제네릭 타입은 제네릭 프로그래밍을 지원하기 위한 기능 중 하나입니다. 제네릭 타입은 다양한 데이터 타입에 대해 유연하게 사용할 수 있는 클래스, 인터페이스, 메서드를 만들 수 있도록 도와줍니다. 제네릭은 코드의 재사용성과 안정성을 높이는데 도움을 줍니다.

제네릭 타입을 정의하려면 다음과 같이 표현합니다:

```java
class 클래스이름<T> {
    // ...
}
```

여기서 `T`는 일반적으로 Type을 나타내는 문자입니다. `T` 대신에 어떤 다른 문자나 단어를 사용할 수도 있습니다.

제네릭을 사용하면 클래스, 인터페이스, 메서드 등을 타입 파라미터로 정의할 수 있습니다. 예를 들어, 다음은 제네릭 클래스의 예입니다:

```java
class Box<T> {
    private T content;

    public T getContent() {
        return content;
    }

    public void setContent(T content) {
        this.content = content;
    }
}
```

위의 `Box` 클래스는 어떤 타입의 객체도 담을 수 있는 상자를 나타냅니다. `T`는 어떤 타입이든 될 수 있으므로 이 클래스를 여러 데이터 타입에 대해 사용할 수 있습니다.

제네릭을 사용하면 다음과 같은 이점을 얻을 수 있습니다:

1. 타입 안정성: 컴파일러는 컴파일 시에 제네릭 타입의 타입 불일치 오류를 검출할 수 있으므로 런타임 오류를 방지할 수 있습니다.

2. 코드 재사용성: 제네릭을 사용하면 같은 로직을 여러 다른 데이터 타입에 대해 재사용할 수 있습니다.

3. 가독성 향상: 제네릭 코드는 타입 변환 코드를 줄여 가독성을 향상시킵니다.

예를 들어, 위에서 정의한 `Box` 클래스를 사용할 때 다음과 같이 타입을 지정할 수 있습니다:

```java
Box<Integer> integerBox = new Box<>();
integerBox.setContent(42);

Box<String> stringBox = new Box<>();
stringBox.setContent("Hello, Generics!");
```

위의 코드에서 `Box` 클래스는 `Integer` 타입과 `String` 타입에 대해 각각 사용됩니다.

이렇게 Java의 Generic Type은 타입 안정성을 높이고 재사용성을 향상시키는데 유용한 기능입니다.
