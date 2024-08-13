# Java의 참조형 객체 - 불변형, 가변형

```java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        Integer a = new Integer(2);   // 불변형 객체 Integer
        List<Integer> list = new ArrayList<>();  // 가변형 객체 List

        list.add(10);

        System.out.println(a);  // 출력: 2
        System.out.println(list.get(0));  // 출력: 10

        sum(a, list);

        System.out.println(a);  // 출력: 2
        System.out.println(list.get(0));  // 출력: 22
    }

    static void sum(Integer a, List<Integer> list) {
        a = 5;  // 불변형 객체의 변경 시 새로운 객체 생성
        list.set(0, 22);  // 가변형 객체의 상태 변경
    }
}

```

**Output:**

```java
2
10
2
22
```

### 불변형 객체(Integer):

Integer a는 불변형 객체입니다. 메서드 sum에서 a = 5로 변경하려고 시도하지만, 이는 새로운 Integer 객체를 생성하여 a가 새로운 객체를 참조하게 됩니다.

따라서, main 메서드에서 a의 값은 여전히 2로 출력됩니다.

### 가변형 객체(List):

List<Integer> list는 가변형 객체입니다. 메서드 sum에서 list.set(0, 22)을 통해 리스트의 첫 번째 값을 변경합니다.

이 변경은 원본 리스트 객체에 반영되므로, main 메서드에서 list.get(0)을 호출하면 22가 출력됩니다.
