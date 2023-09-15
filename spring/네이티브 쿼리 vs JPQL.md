# 네이티브 쿼리(Native Query) vs JPQL(Java Persistence Query Language)

### 문법 및 작성 방식

- **네이티브 쿼리**:
  - SQL 문법을 사용하며 데이터베이스 테이블과 열을 직접 참조.
  - SQL 문장은 문자열로 작성.

- **JPQL**:
  - 객체 지향 쿼리 언어로, 엔터티 클래스와 엔터티의 속성을 기반으로 쿼리 작성.
  - SQL과는 다른 문법 사용.

### 데이터베이스 종속성

- **네이티브 쿼리**:
  - 특정 데이터베이스 시스템의 SQL 문법 사용.
  - 다른 데이터베이스에서는 동일한 쿼리가 작동하지 않을 수 있음.

- **JPQL**:
  - JPA의 일부로, 데이터베이스에서 독립적.
  - JPA가 지원하는 다양한 데이터베이스 시스템에서 사용 가능.

### 객체 지향성

- **네이티브 쿼리**:
  - SQL 중심이며 객체 지향적인 개념을 직접 지원하지 않음.
  - 결과 집합은 엔터티 객체가 아닌 테이블 레코드의 집합.

- **JPQL**:
  - 객체 지향적인 개념을 반영하며, 엔터티 클래스와 객체 간의 관계를 쿼리에서 활용 가능.
  - 결과는 엔터티 객체의 컬렉션으로 반환.

### 유지보수 및 타입 안정성

- **네이티브 쿼리**:
  - 문자열로 작성되며, 컴파일 시점에 구문 오류를 찾기 어려움.
  - 엔터티 클래스 변경 시 쿼리도 수동으로 수정 필요.

- **JPQL**:
  - 컴파일 시점에 구문 오류 쉽게 발견 가능.
  - 엔터티 클래스 변경 시 자동으로 쿼리 조정.

### 보안 및 SQL 인젝션

- **네이티브 쿼리**:
  - 잘못된 사용 시 SQL 인젝션 공격 가능성 존재.

- **JPQL**:
  - 파라미터 바인딩을 통해 SQL 인젝션 공격 방지.

### 예시 코드

- **Entity로 반환**:

  ```java
  // Entity로 반환하는 네이티브 쿼리 예시 코드
  @Query(value = "SELECT * FROM my_table WHERE column_name = 'value'", nativeQuery = true)
  List<MyEntity> nativeQueryExample();
  ```

  ```java
  // Entity로 반환하는 JPQL 예시 코드
  @Query("SELECT e FROM MyEntity e WHERE e.columnName = 'value'")
  List<MyEntity> jpqlExample();
  ```
- **DTO로 반환**:
  ```java
  // DTO로 반환하는 네이티브 쿼리 예시 코드
  @Query(value = "SELECT column1, column2 FROM my_table WHERE column_name = 'value'", nativeQuery = true)
  List<MyDto> nativeQueryDtoExample();
  ```

  ```java
  // DTO로 반환하는 JPQL 예시 코드
  @Query("SELECT new com.example.MyDto(e.property1, e.property2) FROM MyEntity e WHERE e.columnName = 'value'")
  List<MyDto> jpqlDtoExample();
  ```






