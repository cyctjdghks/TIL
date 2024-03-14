# [Java] 배열 정렬하기java

## 1. int 배열 정렬 (오름차순, 내림차순)

`Arrays.sort()`로 int 배열을 인자로 전달하면 오름차순으로 정렬됩니다.

- `sort()` 함수 내부에서 변수 arr의 순서를 변경해주기 때문에 리턴 값을 다른 변수에 할당할 필요가 없음
- 원본 배열의 순서가 변경됨

```java
int[] arr = {1, 26, 17, 25, 99, 44, 303};

Arrays.sort(arr);

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [1, 17, 25, 26, 44, 99, 303]
```

### 내림차순 정렬

내림차순으로 정렬하려면 `sort()`의 인자에 추가로 `Collections.reverseOrder()`를 전달해야 합니다.

- `Collections.reverseOrder()`는 Comparator 객체이며, 역순으로 정렬해줌
- Comparator는 직접 구현할 수 있지만, 미리 정의된 Collections 함수를 사용할 수 있음

```java
Integer[] arr = {1, 26, 17, 25, 99, 44, 303};

Arrays.sort(arr, Collections.reverseOrder());

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [303, 99, 44, 26, 25, 17, 1]
```

### 내림차순 Comparator 직접 구현

```java
Integer[] arr = {1, 26, 17, 25, 99, 44, 303};

Arrays.sort(arr, new Comparator<Integer>() {
    @Override
    public int compare(Integer i1, Integer i2) {
        return i2 - i1;
    }
});

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

또는 람다 표현식을 사용하여:

```java
Integer[] arr = {1, 26, 17, 25, 99, 44, 303};

Arrays.sort(arr, (i1, i2) -> i2 - i1);

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

## 2. int 배열, 부분 정렬

```java
int[] arr = {1, 26, 17, 25, 99, 44, 303};

Arrays.sort(arr, 0, 4);

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [1, 17, 25, 26, 99, 44, 303]
```

## 3. String 배열 정렬(Sorting)

```java
String[] arr = {"Apple", "Kiwi", "Orange", "Banana", "Watermelon", "Cherry"};

Arrays.sort(arr);

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [Apple, Banana, Cherry, Kiwi, Orange, Watermelon]
```

### String 배열, 문자열 길이 순서로 정렬

```java
String[] arr = {"Apple", "Kiwi", "Orange", "Banana", "Watermelon", "Cherry"};

Arrays.sort(arr, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});

System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [Kiwi, Apple, Orange, Banana, Cherry, Watermelon]
```

## 4. 객체 배열 정렬(Sorting)

```java
public static class Fruit implements Comparable<Fruit> {
    private String name;
    private int price;

    public Fruit(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String toString() {
        return "{name: " + name + ", price: " + price + "}";
    }

    @Override
    public int compareTo(@NotNull Fruit fruit) {
        return this.price - fruit.price;
    }
}

Fruit[] arr = {
    new Fruit("Apple", 100),
    new Fruit("Kiwi", 500),
    new Fruit("Orange", 200),
    new Fruit("Banana", 50),
    new Fruit("Watermelon", 880),
    new Fruit("Cherry", 10)
};

Arrays.sort(arr);
System.out.println("Sorted arr[] : " + Arrays.toString(arr));
```

**Output:**

```java
Sorted arr[] : [{name: Cherry, price: 10}, {name: Banana, price: 50}, {name: Apple, price: 100}, {name: Orange, price: 200}, {name: Kiwi, price: 500}, {name: Watermelon, price: 880}]
```
