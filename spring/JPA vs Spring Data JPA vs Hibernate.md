# JPA vs Spring Data JPA vs Hibernate

- JPA (Java Persistence API): JPA는 자바 표준 ORM 기술로, Java EE (Enterprise Edition)에서 정의된 인터페이스의 모음입니다. JPA는 데이터베이스와의 상호작용을 추상화하여 개발자가 객체지향적인 방식으로 데이터베이스에 접근하고 조작할 수 있게 해줍니다. JPA는 데이터베이스와의 연결, 객체와 테이블 간의 매핑, 객체의 상속, 관계 매핑, 트랜잭션 관리 등을 다룹니다.

- Hibernate: Hibernate는 JPA의 구현체 중 하나로, JPA 스펙을 구현한 오픈소스 ORM 프레임워크입니다. Hibernate는 JPA 스펙을 준수하면서 추가적인 확장 기능들을 제공하며, 많은 개발자들이 널리 사용하는 ORM 프레임워크 중 하나입니다. Hibernate는 JPA의 인터페이스를 구현하여 데이터베이스와의 상호작용을 처리하고, 객체와 테이블 간의 매핑, 관계 매핑, 캐싱, 성능 최적화 등의 기능을 제공합니다.

- Spring Data JPA: Spring Data JPA는 스프링 프레임워크의 일부로, JPA를 더 쉽게 사용할 수 있도록 도와주는 기술입니다. Spring Data JPA는 JPA를 기반으로 하여 데이터 액세스 레이어를 보다 쉽게 개발할 수 있도록 도와주는 프레임워크로, JPA의 스펙을 구현하는 Hibernate 등의 ORM 프레임워크와 함께 사용될 수 있습니다. Spring Data JPA는 CRUD (Create, Read, Update, Delete) 작업의 메소드를 자동으로 생성해주는 기능, 동적 쿼리 작성을 위한 기능, 페이징 및 정렬 처리를 위한 기능 등을 제공하여 개발자가 효율적으로 데이터 액세스를 구현할 수 있게 해줍니다.

JPA는 자바 표준 ORM 기술이며, Hibernate는 JPA의 구현체 중 하나로, Spring Data JPA는 스프링 프레임워크에서 JPA를 더 쉽게 사용할 수 있도록 도와주는 프레임워크. Hibernate와 Spring Data JPA는 JPA 스펙을 준수하면서 확장 기능들을 제공하며, 개발자가 객체지향적인 방식으로 데이터베이스와 상호작용하고 데이터 액세스 레이어를 효율적으로 개발할 수 있도록 도와준다.
