# JPA Entity

### Entity 선언 시 primitive type(int) 대신 wrapper class(Integer) 로 선언하는 이유
- 자료형 vs 와퍼 클래스 차이점
![image](./img/Wrapper%20Classes.jpg)
  - 자료형(primitive type)
    - 산술 연산 가능함
    - null로 초기화 불가
  - 래퍼 클래스 (Wrapper class)
    - Unboxing하지 않을 시 산술 연산 불가능함
    - null값 처리 가능<br>

- wrapper class 로 선언하는 이유
  - JPA에서 null 값 처리를 위해서
    - JPA에서는 데이터베이스의 NULL 값을 처리하는 방법을 정의할 수 있습니다. 예를 들어, @Column(nullable = false) 어노테이션을 사용하면 해당 필드가 null 값을 허용하지 않음을 나타낼 수 있습니다. 그러나 이는 primitive type인 int에서는 사용할 수 없습니다. 그렇기 때문에 Integer를 사용하면 null 값을 처리할 수 있는 더 많은 옵션을 사용할 수 있습니다.

```java
import javax.persistence.*;

@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "name")
    private String name;

    @Column(name = "age")
    private Integer age;

}
```
