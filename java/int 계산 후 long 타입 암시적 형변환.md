### int 계산 후 long 타입 암시적 형변환

```java
public class Test {
    public static void main(String[] args) {
        int a = Integer.MAX_VALUE;  // int의 최대값, 2147483647

        long b = a + 1;  // a + 1 계산 후 결과를 long 타입의 b에 저장
        System.out.println(a);  // 출력: 2147483647
        System.out.println(b);  // 출력: -2147483648
    }
}
```

**Output:**

```java
2147483647
-2147483648
```

- **연산 순서** : a + 1 연산이 먼저 int 타입으로 수행됩니다.
- **오버플로우 발생** : 2147483647 + 1이 int 타입의 최대 범위를 초과하므로 오버플로우가 발생하여 결과가 -2147483648이 됩니다.
- **암시적 형변환** : 이 -2147483648 값을 long 타입에 할당할 때 자바는 자동으로 형변환을 수행합니다. 하지만 이 과정에서는 값의 변동이 없으므로, b의 값은 -2147483648로 저장됩니다.
