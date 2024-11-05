# [Java] 형변환

## int & double

### int to double

```java
int a = 100;
long b = a;       // int -> long
double c = b;     // long -> double
```

### double to int

```java
double a = 100.5;
int b = (int) a;  // double -> int, 값은 100
```

---

## int & String

### int to String

```java
int a = 100;
String str = String.valueOf(a);  // int -> String
```

### String to int

```java
String str = "100";
int a = Integer.parseInt(str);    // String -> int
```

---

## char & String

### char to String

```java
char c = 'A';
String str = String.valueOf(c);  // char -> String
```

### String to char

```java
String str = "Hello";
char c = str.charAt(0);          // 첫 번째 문자 'H'
```

---

## char & int

### char to int

```java
char c = 'A';
int asciiValue = (int) c;       // 'A'의 ASCII 값 65
```

### int to char

```java
int asciiValue = 66;
char c = (char) asciiValue;     // ASCII 값 66 -> 문자 'B'
```

---

**참고:**

- 대문자 'A'부터 'Z'의 ASCII 값은 65부터 90.
- 소문자 'a'부터 'z'의 ASCII 값은 97부터 122.
