## Spring `@ModelAttribute` vs `@RequestParam`

### 1. `@ModelAttribute`

**역할**

- HTTP 요청 파라미터를 Java 객체에 바인딩합니다.
- 바인딩된 객체를 Model에 자동 등록합니다.

**동작 방식**

- 요청 파라미터 이름과 Java 객체의 필드명이 일치하면 setter를 통해 자동으로 바인딩됩니다.

**적합한 사용 사례**

- 여러 요청 파라미터를 DTO로 묶어 처리해야 할 때
- HTML Form 데이터를 바인딩할 때

**예시 요청**
`GET /search?username=alice&age=30`

**예시 코드**

```java
@GetMapping("/search")
public String search(@ModelAttribute UserSearchDto dto) {
    // dto.getUsername() -> "alice"
    // dto.getAge() -> 30
    return "searchResult";
}
```

---

### 2. `@RequestParam`

**역할**

- 단일 요청 파라미터를 메서드 인자로 바인딩합니다.

**동작 방식**

- 요청 파라미터 이름과 메서드 인자 이름이 일치하면 자동 바인딩됩니다.

**적합한 사용 사례**

- 간단한 쿼리 파라미터 처리 (예: 페이지 번호, 검색어 등)

**예시 요청**
`GET /search?username=alice&age=30`

**예시 코드**

```java
@GetMapping("/search")
public String search(@RequestParam String username, @RequestParam int age) {
    // username -> "alice"
    // age -> 30
    return "searchResult";
}
```

---

## 생략 시 스프링의 기본 처리 규칙

파라미터에 애노테이션을 생략했을 때 스프링은 다음과 같은 규칙을 적용합니다.

- 기본 타입 (`String`, `int`, `Integer` 등) → `@RequestParam`이 기본 적용됨
- 사용자 정의 클래스 등 복합 타입 → `@ModelAttribute`가 기본 적용됨
- 단, `HttpServletRequest`, `Model`, `BindingResult` 등 ArgumentResolver에 의해 처리되는 타입은 제외

예시:

```java
// username -> @RequestParam, userDto -> @ModelAttribute로 처리됨
public String submit(String username, UserDto userDto) {
    ...
}
```

---

## `@ModelAttribute`의 장점

- 요청 파라미터를 객체로 묶어 구조화할 수 있어 유지보수에 유리함
- 폼 데이터 처리 시 자연스럽게 연동됨
- 유효성 검증 및 바인딩 오류 처리에 용이함 (`@Valid`, `BindingResult`와 조합)

---

### `@Valid`

- Bean Validation(JSR-380) 기반 유효성 검증을 수행함
- 객체의 필드에 제약 조건을 선언하면 자동으로 검증됨

예시 DTO:

```java
public class User {
    @NotBlank
    private String username;

    @Min(18)
    private int age;
}
```

---

### `BindingResult`

- 유효성 검사 실패 및 바인딩 오류 정보를 담는 객체
- `@Valid` 바로 뒤에 선언해야 정상 동작함
- 오류가 발생해도 예외가 던져지지 않고 직접 처리 가능함

예시:

```java
@PostMapping("/register")
public String register(@ModelAttribute @Valid User user, BindingResult result) {
    if (result.hasErrors()) {
        return "registerForm";
    }
    return "redirect:/success";
}
```

> 참고: `BindingResult`는 서버사이드 렌더링(SSR) 기반에서만 유효하며, REST API 환경에서는 글로벌 예외 처리기를 사용하는 것이 일반적입니다.

---

## REST API 구조에서는 글로벌 예외 처리 방식 사용

예시:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleValidationException(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
            errors.put(error.getField(), error.getDefaultMessage())
        );
        return ResponseEntity.badRequest().body(errors);
    }
}
```

Controller:

```java
@PostMapping("/api/users")
public ResponseEntity<?> createUser(@RequestBody @Valid UserDto dto) {
    userService.save(dto);
    return ResponseEntity.ok().build();
}
```

---

## 핵심 비교 정리

| 항목             | @ModelAttribute         | @RequestParam            |
| ---------------- | ----------------------- | ------------------------ |
| 바인딩 대상      | 복합 타입 객체 (DTO 등) | 단일 값 (String, int 등) |
| 기본 적용 대상   | 복합 타입               | 단순 타입                |
| 유효성 검사 지원 | 가능 (`@Valid` 사용)    | 불가능                   |
| 모델 자동 등록   | 자동으로 등록됨         | 등록되지 않음            |
| 대표적 용도      | 폼 데이터 바인딩        | 쿼리 파라미터 바인딩     |
