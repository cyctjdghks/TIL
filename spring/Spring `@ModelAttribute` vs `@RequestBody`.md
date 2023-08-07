## Spring `@ModelAttribute` vs `@RequestBody`

### 1. `@ModelAttribute`

**역할**: 
- HTTP 요청 파라미터나 폼 데이터를 Java 객체에 바인딩합니다.

**동작 방식**:
- 요청 파라미터나 폼 데이터의 키와 Java 객체의 필드 이름을 기반으로 바인딩합니다.
- 데이터 타입 변환, 검증, 에러 처리 등을 지원합니다.

**적합한 사용 사례**: 
- GET 요청의 URL 쿼리 파라미터
- POST 요청의 `application/x-www-form-urlencoded` 형태의 폼 데이터

**예시**:

- **GET 요청 (URL 쿼리 파라미터)**
```plaintext
GET /search?name=John&email=john@example.com
```

Controller:
```java
@GetMapping("/search")
public String search(@ModelAttribute User user) {
    // ...
}
```

- **POST 요청 (폼 데이터)**
```html
<form action="/register" method="post">
    <label>Name:</label>
    <input type="text" name="name" />
    <label>Email:</label>
    <input type="text" name="email" />
    <input type="submit" value="Register" />
</form>
```

Controller:
```java
@PostMapping("/register")
public String register(@ModelAttribute User user) {
  // ...
}
```

### 2. `@RequestBody`

**역할**: 
- HTTP 요청의 본문(body)을 Java 객체로 변환합니다.

**동작 방식**:
- HTTP 요청 본문의 내용을 읽고 적절한 메시지 컨버터를 사용하여 Java 객체로 변환합니다. (예: JSON → Java 객체)

**적합한 사용 사례**: 
- RESTful 웹 서비스에서 JSON, XML 등의 복잡한 데이터 구조를 처리할 때

**예시**:
```plaintext
POST /api/users
Content-Type: application/json

{
  "name": "John",
  "email": "john@example.com"
}
```

Controller:
```java
@PostMapping("/api/users")
public ResponseEntity<?> createUser(@RequestBody User user) {
    // ...
}
```