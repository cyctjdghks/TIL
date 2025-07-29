# Spring Security PasswordEncoder 종류

Spring Security에서는 비밀번호를 안전하게 저장하고 검증하기 위해 다양한 `PasswordEncoder` 구현체를 제공합니다. 각 인코더는 보안 수준, 성능, 사용 목적에 따라 선택할 수 있습니다.

## 주요 PasswordEncoder 비교

| 암호화 방식    | 클래스명                    | 보안성 | 속도    | 메모리 사용 | 특징                                   | 사용 예시                     |
| -------------- | --------------------------- | ------ | ------- | ----------- | -------------------------------------- | ----------------------------- |
| **BCrypt**     | `BCryptPasswordEncoder`     | ★★★★☆  | 중      | 낮음        | 자동 salt, 해싱 횟수 조절 가능         | 일반적인 웹 서비스            |
| **PBKDF2**     | `Pbkdf2PasswordEncoder`     | ★★★★☆  | 빠름~중 | 낮음        | 반복 기반 해싱, HMAC 지원              | 보안 요구사항 높은 서비스     |
| **SCrypt**     | `SCryptPasswordEncoder`     | ★★★★★  | 느림    | 높음        | 메모리 기반 연산, 공격자 비용 높음     | 핀테크, 의료 등 고보안 시스템 |
| **Argon2**     | `Argon2PasswordEncoder`     | ★★★★★  | 느림    | 높음        | 최신 암호화 알고리즘, 병렬성 설정 가능 | 최신 보안 표준 적용 서비스    |
| **Delegating** | `DelegatingPasswordEncoder` | -      | -       | -           | 다중 인코더 지원, 마이그레이션에 유용  | 기존 데이터 이관 시           |
| **NoOp**       | `NoOpPasswordEncoder`       | ✖      | 빠름    | 없음        | 암호화 없음 (테스트 전용)              | 로컬 테스트 용도만 사용       |

---

## 실제 예시 코드: PasswordEncoder 사용법

회원가입 시 비밀번호를 암호화하여 저장하고, 로그인 시 입력한 비밀번호와 DB의 암호화된 값을 비교합니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(); // 또는 Argon2, SCrypt 등으로 교체 가능
    }
}
```

---

### 1. 비밀번호 암호화 (회원가입 시)

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final PasswordEncoder passwordEncoder;
    private final UserRepository userRepository;

    public void register(String username, String rawPassword) {
        String encodedPassword = passwordEncoder.encode(rawPassword); // 암호화
        userRepository.save(new User(username, encodedPassword)); // 저장
    }
}
```

---

### 2. 비밀번호 검증 (로그인 시)

```java
@Service
@RequiredArgsConstructor
public class LoginService {

    private final PasswordEncoder passwordEncoder;
    private final UserRepository userRepository;

    public boolean login(String username, String inputPassword) {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found"));

        return passwordEncoder.matches(inputPassword, user.getPassword()); // 검증
    }
}
```

---

### 주의: 이렇게 하면 안 됨

```java
// 잘못된 방식
passwordEncoder.encode(inputPassword).equals(user.getPassword());
```

- `encode()`는 매번 결과가 다르기 때문에 직접 비교하면 항상 false가 나옵니다.
- 반드시 `matches()` 메서드를 사용해야 합니다.

---

### 결과 흐름 요약

| 상황        | 메서드                        | 설명                           |
| ----------- | ----------------------------- | ------------------------------ |
| 회원가입 시 | `encode(rawPassword)`         | 평문 비밀번호 → 암호화 후 저장 |
| 로그인 시   | `matches(raw, encodedFromDb)` | 입력한 비밀번호 일치 여부 검증 |

---
