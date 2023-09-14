# Gradle의 sourceSets와 Spring 프로파일(Profile)

Gradle 빌드 스크립트와 Spring 애플리케이션의 설정에서 사용되는 `sourceSets`, 프로파일(Profile), 그리고 실행 시 VM 옵션으로 `spring.profiles.active` 설정을 지정하는 방법에 대한 설명입니다.

## sourceSets

### 목적

`sourceSets`는 Gradle 빌드 스크립트에서 사용되며 프로젝트의 소스 코드 및 리소스 디렉토리를 정의하고 구성하는 데 사용됩니다.

### 사용 레벨

- `sourceSets`는 프로젝트의 구조를 정의하고 빌드 프로세스에 어떤 디렉토리가 어떻게 사용될지를 지정하는 데 사용됩니다. 
- 주로 Java 또는 Kotlin 프로젝트에서 사용되며, 소스 코드 및 리소스의 배치와 관련된 많은 빌드 태스크 및 설정에 영향을 미칩니다.

### 구성 방법

- `sourceSets`는 Gradle 빌드 스크립트 내에서 정의되며, `java.srcDirs`, `resources.srcDirs` 등과 같은 속성을 사용하여 각 소스 세트의 디렉토리를 지정합니다.

### 예시

```groovy
sourceSets {
    main {
        java.srcDirs 'src/main/java'
        resources.srcDirs 'src/main/resources'
    }
    test {
        java.srcDirs 'src/test/java'
        resources.srcDirs 'src/test/resources'
    }
}
```

## 프로파일(Profile)

### 목적

프로파일은 Spring과 같은 프레임워크에서 사용되며, 애플리케이션의 실행 시점에 필요한 설정을 선택하는 데 사용됩니다. 각 프로파일은 특정 환경 또는 설정을 나타냅니다.

### 사용 레벨

- 프로파일은 애플리케이션 실행 동작을 제어하는 데 사용됩니다.
- 주로 `application.properties` 또는 `application.yml` 파일에서 설정되며, `spring.profiles.active` 속성을 통해 활성화할 프로파일을 지정합니다.

### 구성 방법

- 프로파일은 Spring 및 다른 프레임워크의 설정 파일에서 정의되며, 설정 파일 내에서 활성화할 프로파일을 명시적으로 지정합니다.

### 예시 (Spring Boot의 `application.properties` 파일)

```properties
spring.profiles.active=local
```

## 실행 시 VM 옵션으로 `spring.profiles.active` 설정

Spring 애플리케이션을 실행할 때, 원하는 프로파일을 설정하기 위해 VM 옵션으로 `spring.profiles.active`을 지정할 수 있습니다. 아래는 실행 시 VM 옵션으로 `local` 프로파일을 활성화하는 예시입니다:

```shell
java -Dspring.profiles.active=local -jar your-application.jar
```

이렇게 하면 애플리케이션을 실행할 때 해당 프로파일의 설정을 사용합니다.

## 정리

- `sourceSets`는 빌드 과정에서 사용할 리소스 목록을 정의하고 Gradle 빌드 스크립트에서 설정됩니다.
- 프로파일(Profile)은 실행 과정에서 필요한 리소스를 선택하고 애플리케이션 실행 중에 설정을 변경합니다.
- 실행 시 VM 옵션으로 `spring.profiles.active`을 설정하여 원하는 프로파일을 활성화할 수 있습니다.
