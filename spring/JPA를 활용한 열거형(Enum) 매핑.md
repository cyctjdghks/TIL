# JPA를 활용한 열거형(Enum) 매핑

JPA 엔터티 클래스에서 열거형(Enum)을 사용하면 데이터베이스 테이블의 특정 컬럼 값을 제한하고 가독성을 높일 수 있습니다. 열거형은 관련된 상수 값을 그룹화하여 코드를 보다 명확하게 만들어줍니다.

예를 들어, 성별 정보를 나타내는 경우 "남자" 또는 "여자"와 같은 두 가지 값을 가질 수 있는데, 이러한 경우에 열거형을 사용하면 코드를 훨씬 이해하기 쉽게 만들 수 있습니다.

예시 Enum
```java
public enum Gender {
    MALE, FEMALE
}
```
## @Enumerated(EnumType.STRING)

`@Enumerated(EnumType.STRING)`은 Spring Data JPA에서 열거형(Enum) 값을 데이터베이스 테이블의 문자열 컬럼과 매핑할 때 사용하는 JPA 어노테이션입니다. 이 어노테이션을 사용하면 열거형 상수가 해당 상수의 이름(문자열)으로 데이터베이스에 저장됩니다. 주로 가독성과 유지보수성을 높이는 목적으로 사용됩니다.

```java
import javax.persistence.Entity;
import javax.persistence.EnumType;
import javax.persistence.Enumerated;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Enumerated(EnumType.STRING)
    private Gender gender;

    // 나머지 필드 및 메서드
}
```

## Attribute Converter

Attribute Converter는 데이터베이스와 엔터티 클래스 사이의 커스텀 변환 로직을 정의할 수 있도록 하는 JPA 기능입니다. 열거형(Enum) 값을 데이터베이스 컬럼의 다른 데이터 타입으로 변환하고, 그 반대로 변환할 때 사용됩니다.

```java
import javax.persistence.AttributeConverter;
import javax.persistence.Converter;

@Converter
public class GenderConverter implements AttributeConverter<Gender, Integer> {

    @Override
    public Integer convertToDatabaseColumn(Gender gender) {
        if (Gender.MALE.equals(gender)) {
            return 1;
        } else if (Gender.FEMALE.equals(gender)) {
            return 2;
        }
        return 0;
    }

    @Override
    public Gender convertToEntityAttribute(Integer code) {
        if (1 == code) {
            return Gender.MALE;
        } else if (2 == code) {
            return Gender.FEMALE;
        }
        return null; // 또는 기본값 설정
    }
}
```

```java
@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Convert(converter = GenderConverter.class)
    private Gender gender;

    // 나머지 필드 및 메서드
}
```