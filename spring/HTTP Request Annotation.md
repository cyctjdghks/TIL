# HTTP Request Annotation

## @PathVariable
- URL 경로에서 중괄호에 명시된 값을 추출하여 변수에 바인딩할 때 사용하는 어노테이션

#### Example
```java
@GetMapping("/users/{userId}")
public ResponseEntity<User> getUser(@PathVariable("userId") Long userId) {
    // userId를 사용하여 사용자 정보를 조회하는 로직
    // ...
    return ResponseEntity.ok(user);
}

```

위의 예시에서는 "/users/{userId}" 경로에서 userId 값을 추출하여 Long 타입의 userId 변수에 바인딩하여 사용자 정보를 조회하는 메서드입니다.<br>
요청을 보낼 때는 URL 경로에 userId 값을 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
GET http://localhost:8080/users/123

## @RequestParam
- HTTP 요청의 파라미터를 변수에 바인딩할 때 사용하는 어노테이션

#### Example
```java
@GetMapping("/users")
public ResponseEntity<List<User>> getUsers(@RequestParam("name") String name, @RequestParam("age") int age) {
    // name과 age 값을 사용하여 사용자 정보를 조회하는 로직
    // ...
    return ResponseEntity.ok(users);
}

```

위의 예시에서는 name과 age라는 이름의 파라미터 값을 String과 int 타입의 변수에 바인딩하여 사용자 정보를 조회하는 메서드입니다.<br>
요청을 보낼 때는 URL에 쿼리 파라미터로 name과 age 값을 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
GET http://localhost:8080/users?name=John&age=30

## @ModelAttribute
- HTTP 요청의 파라미터를 객체에 바인딩하고 뷰에 전달할 때 사용하는 어노테이션

#### Example
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@ModelAttribute User user) {
    // user 객체를 사용하여 사용자 정보를 생성하는 로직
    // ...
    return ResponseEntity.ok(createdUser);
}

```

위의 예시에서는 HTTP 요청의 파라미터를 User 객체에 바인딩하여 사용자 정보를 생성하는 메서드입니다.<br>
요청을 보낼 때는 요청 본문에 User 객체의 필드를 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
POST http://localhost:8080/users<br>
Body :
```json
{
  "name": "John",
  "age": 30
}

```

## @RequestBody
- HTTP 요청의 바디를 객체에 바인딩할 때 사용하는 어노테이션
- XML, JSON 일떄 이것을 주로 사용

#### Example
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User user) {
    // user 객체를 사용하여 사용자 정보를 생성하는 로직
    // ...
    return ResponseEntity.ok(createdUser);
}

```

위의 예시에서는 HTTP 요청의 바디를 User 객체에 바인딩하여 사용자 정보를 생성하는 메서드입니다.<br>
요청을 보낼 때는 요청 본문에 User 객체의 필드를 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
POST http://localhost:8080/users<br>
Body:
```json
{
  "name": "John",
  "age": 30
}

```

## @RequestPart
- HTTP 멀티파트 요청의 파트를 변수에 바인딩할 때 사용하는 어노테이션

#### Example
```java
@PostMapping("/upload")
public ResponseEntity<String> uploadFile(@RequestPart("file") MultipartFile file) {
    // 업로드된 파일을 처리하는 로직
    // ...
    return ResponseEntity.ok("File uploaded successfully");
}

```

위의 예시에서는 @RequestPart 어노테이션을 사용하여 "file"이라는 이름의 멀티파트 파트를 MultipartFile 타입의 변수에 바인딩하여 파일 업로드를 처리하는 메서드입니다.<br>
요청을 보낼 때는 멀티파트 형태로 파일을 첨부하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
POST http://localhost:8080/upload<br>
Body:
```vbnet
[Select "form-data" in Body tab in Postman]
[Add "file" as Key and Select "File" as Type]
[Choose a file to upload]
```

## @RequestHeader
- HTTP 요청의 헤더를 변수에 바인딩할 때 사용하는 어노테이션

#### Example
```java
@GetMapping("/users")
public ResponseEntity<List<User>> getUsers(@RequestHeader("Authorization") String token) {
    // 헤더에서 Authorization 값을 추출하여 토큰을 처리하는 로직
    // ...
    return ResponseEntity.ok(users);
}

```

위의 예시에서는 @RequestHeader 어노테이션을 사용하여 "Authorization"이라는 이름의 헤더 값을 String 타입의 변수에 바인딩하여 인증 토큰을 처리하는 메서드입니다.<br>
요청을 보낼 때는 헤더에 Authorization 값을 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
GET http://localhost:8080/users<br>
Headers:
```css
Authorization: Bearer {token}
```

## @CookieValue
- HTTP 요청의 쿠키 값을 변수에 바인딩할 때 사용하는 어노테이션

#### Example
```java
@GetMapping("/products")
public ResponseEntity<List<Product>> getProducts(@CookieValue("userId") String userId) {
    // 쿠키에서 userId 값을 추출하여 처리하는 로직
    // ...
    return ResponseEntity.ok(products);
}

```

위의 예시에서는 @CookieValue 어노테이션을 사용하여 "userId"라는 이름의 쿠키 값을 String 타입의 변수에 바인딩하여 사용자 아이디를 처리하는 메서드입니다.<br>
요청을 보낼 때는 쿠키에 userId 값을 포함하여 요청을 보낼 수 있습니다.<br>

요청 예시:<br>
GET http://localhost:8080/products<br>
Headers:
```makefile
Cookie: userId=12345
```
