# Spring JPA Entity Default 설정

## **1. `@ColumnDefault`**

```java
import org.hibernate.annotations.ColumnDefault;

@Entity
public class MyEntity {
   @ColumnDefault("Default Name")
   private String name;
   
   // ...
}
```

`@ColumnDefault`는 Hibernate JPA 애노테이션으로, 데이터베이스 테이블의 열에 기본값을 설정합니다. 위 예시에서 `name` 필드의 데이터베이스 열에 "Default Name"이 기본값으로 설정됩니다.


## **2. `@Builder.Default`**

```java
import lombok.Builder;

@Entity
public class MyEntity {
   @Builder.Default
   private String name = "Default Name";
   
   // ...
}
```

`@Builder.Default`는 Lombok 애노테이션으로, 해당 필드의 기본값을 설정하며 객체를 빌더 패턴을 통해 생성할 때 사용됩니다. 위 예시에서 `name` 필드는 "Default Name"으로 기본값이 설정됩니다.