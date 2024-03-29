# Spring MVC

## Servlet (서블릿)
웹 어플리케이션 서버 요청 응답 구조
(내장 톰캣 서버는 스프링 부트에서 편리하게 자동으로 생성합니다.)

- 클라이언트에서 URL로 요청을 보냅니다.
- 웹 어플리케이션 서버(WAS)는 메시지를 기반으로 Request 객체를 생성하고, Response 객체도 생성합니다.
- 서블릿 컨테이너에서 WAS는 두 객체를 받아 실행한 뒤, 처리가 끝나면 Response를 클라이언트에 반환합니다.

## 클라이언트에서 서버로 데이터를 전달하는 방법
3가지 방법이 있습니다.

- 쿼리 파라미터
- HTML FORM
- Message Body에 JSON, 텍스트 등의 데이터를 직접 담아 요청합니다.
강의에서는 이 3가지를 자바 코드로 다루는 것을 배우고, 나중에 스프링이 이 기능을 간편하게 제공합니다.

## 응답 메시지
응답 메시지는 아래 3가지로 구성됩니다.

- HTTP 응답 코드
- 헤더
- 바디

이 3가지는 API를 개발할 때 중요하게 다루는 요소입니다.

## MVC 패턴
### 이 패턴이 생긴 이유
하나의 서블릿이나 JSP로 모든 로직과 렌더링을 처리하면 다음과 같은 문제가 발생합니다.

1. 유지 보수가 어려워집니다.
2. 비즈니스 로직과 UI가 뒤섞여 복잡해집니다.
3. 병렬적 처리가 어려워집니다.

### MVC란?
MVC는 Model, View, Controller의 약자로 아래와 같은 역할을 합니다.

- Controller: HTTP 요청을 받아 파라미터를 검증하고, 비즈니스 로직을 실행한 후 결과 데이터를 모델에 담습니다.
- Model: 뷰에 출력할 데이터를 보유하며, 뷰에 독립적으로 데이터를 관리합니다.
- View: 모델에 담긴 데이터를 이용하여 화면을 렌더링합니다.

### Redirect vs Forward
- Redirect: 클라이언트에게 응답을 보낸 뒤, 클라이언트가 다시 요청을 하며 URL도 변경됩니다.
- Forward: 서버 내부에서 처리되며, 클라이언트는 이를 인지하지 못합니다.

### 순수 MVC 패턴의 단점
순수한 MVC 패턴은 아래와 같은 단점을 가지고 있습니다.

1. 중복 코드가 많아집니다.
2. 사용하지 않는 코드도 작성될 수 있습니다.
3. 공통된 부분을 처리하기가 어려워집니다.

Front Controller 패턴을 도입해 이러한 문제를 해결하고, 스프링 MVC는 이 패턴을 기반으로 핵심 기능을 제공합니다.

### Front Controller 패턴
Front Controller 패턴은 웹 애플리케이션에서 모든 요청을 한 곳에서 집중적으로 처리하는 패턴입니다. 이렇게 하면 중복 코드를 줄이고 공통된 작업을 처리할 수 있습니다. 스프링 MVC도 Front Controller 패턴을 사용하며, DispatcherServlet이 해당 역할을 담당합니다.

## 스프링 MVC
### 스프링 MVC 구조
1. 핸들러 매핑을 통해 요청 URL에 맞는 컨트롤러를 찾습니다.
2. 핸들러 어댑터를 찾아 해당 컨트롤러에 데이터를 전달합니다.
3. 컨트롤러가 데이터를 처리한 뒤, 결과를 ModelAndView로 반환합니다.
4. ViewResolver가 뷰의 논리 이름을 실제 물리 이름으로 변경하여 뷰를 찾아 반환합니다.
5. View에서 Model이 렌더링됩니다.

### 스프링 MVC 기능들 정리
#### 컨트롤러
- @Controller: 스프링 빈으로 자동 등록되며, 어노테이션 기반 컨트롤러로 인식됩니다.
- @RestController: REST API를 위한 어노테이션으로, @ResponseBody와 결합하여 사용합니다.

#### 매핑
- @GetMapping: GET 요청을 처리하는 핸들러 매핑 어노테이션
- @PostMapping: POST 요청을 처리하는 핸들러 매핑 어노테이션
- @PutMapping: PUT 요청을 처리하는 핸들러 매핑 어노테이션
- @PatchMapping: PATCH 요청을 처리하는 핸들러 매핑 어노테이션
- @DeleteMapping: DELETE 요청을 처리하는 핸들러 매핑 어노테이션

#### 파라미터
- @RequestParam: URL 파라미터를 처리합니다.
- @ModelAttribute: 요청 파라미터를 객체에 매핑해 줍니다.

#### 로그
- Lombok의 @Slf4j를 사용하면 로그 객체 생성이 간편합니다.
- 로그 레벨에 따라 출력되는 내용이 달라집니다.
