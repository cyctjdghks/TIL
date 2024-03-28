# QueryDSL 설정

QueryDSL 은 gradle 에다가 설정하고 gradle 빌드 과정 속에서 Q타입이라는 파일을 뽑아내야한다.

### 플러그인 추가

- 스프링 부트 3.x.x

```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.0'
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'study'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    //test 롬복 사용
    testCompileOnly 'org.projectlombok:lombok'
    testAnnotationProcessor 'org.projectlombok:lombok'
    //Querydsl 추가
    implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
    annotationProcessor "com.querydsl:querydsl-apt:$
    {dependencyManagement.importedProperties['querydsl.version']}:jakarta"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api"
    annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}

tasks.named('test') {
    useJUnitPlatform()
}

clean {
delete file('src/main/generated')
}
```

- 스프링 부트 2.x.x

```java
plugins {
    ...
	//querydsl 추가
	id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
    ...
}

...

dependencies {
    ...

	//querydsl 추가
	implementation 'com.querydsl:querydsl-jpa'

    ...
}

...

//querydsl 추가 시작
def querydslDir = "$buildDir/generated/querydsl"
querydsl {
	jpa = true
	querydslSourcesDir = querydslDir
}
sourceSets {
	main.java.srcDir querydslDir
}
configurations {
	querydsl.extendsFrom compileClasspath
}
compileQuerydsl {
	options.annotationProcessorPath = configurations.querydsl
}
//querydsl 추가 끝
```

- 스프링 부트 2.6.x 버전 이상 (참고 사이트 : https://velog.io/@senglo/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-2.6-%EC%9D%B4%EC%83%81%EB%B6%80%ED%84%B0-querydsl-gradle-%EC%84%A4%EC%A0%95)

```java
// 스프링 부트 2.6 이상 부터는 querydsl 5.0을 사용한다
buildscript {
	ext {
		queryDslVersion = "5.0.0"
	}
}

plugins {
	...
	// QueryDSL 모델 파일을 생성하기 위한 플러그인
	id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
	...
}

...

dependencies {
	...

    // 5.0 이전 버전에는 'com.querydsl:querydsl-jpa' 의존성만 추가하면 됐지만
    // 5.0 이후 버전에는 3개의 프로젝트로 갈라진거 같다.
	// querdysl ${querydDslVersion} 은 맨위에 선언한 5.0 변수를 사용함
	implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
	implementation "com.querydsl:querydsl-apt:${queryDslVersion}"
	implementation "com.querydsl:querydsl-core:${queryDslVersion}"

	...
}

...

// querydsl 설정

// querydsl 빌드 경로 변수
def querydslDir = "$buildDir/generated/querydsl"

querydsl {
	jpa = true
	querydslSourcesDir = querydslDir
}

// build 시 사용할 sourceSet 추가
sourceSets {
	main.java.srcDir querydslDir
}

configurations {
	querydsl.extendsFrom compileClasspath
}

// querydsl 컴파일시 사용할 옵션 설정
compileQuerydsl{
	options.annotationProcessorPath = configurations.querydsl
}
```

### QEntity 생성

1. Entity 생성

2. Gradle 창의 Tasks - other - compileQuerydsl 클릭
   ![image](../img/QEntity%20생성.png)

3. `./gradlew compileQuerydsl` 혹은 `./gradlew compileJava` 명령어 실행

### QEntity 확인

/build 폴더 하위에서 확인 가능

![image](../img/QEntity%20확인.png)

### 예시 test 코드

```java
import com.querydsl.jpa.impl.JPAQueryFactory;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;
import study.querydsl.entity.Hello;
import study.querydsl.entity.QHello;

import javax.persistence.EntityManager;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootTest
@Transactional
class QuerydslApplicationTests {

	@Autowired
	EntityManager em;

	@Test
	void contextLoads() {
		Hello hello = new Hello();
		em.persist(hello);

		JPAQueryFactory query = new JPAQueryFactory(em);
		QHello qHello = new QHello("h");

		Hello result = query
				.selectFrom(qHello)
				.fetchOne();

		assertThat(result).isEqualTo(hello);
		assertThat(result.getId()).isEqualTo(hello.getId());
	}

}

```
