# Spring MVC

**목차**  
[Spring MVC](#spring-mvc)
- [Model](#model)
- [View](#view)
- [Controller](#controller)
- [MVC Model 1 아키텍처](#mvc-model-1-아키텍처)
- [MVC Model 2 아키텍처](#mvc-model-2-아키텍처)
- [Spring MVC 구성요소](#spring-mvc-구성요소)
- [DispatcherServlet 내부 동작 흐름](#dispatcherservlet-내부-동작-흐름)

## **Spring MVC**

Spring 프레임워크는 모듈 중 하나인 Web 모듈에 MVC Model 2 아키텍처의 발전형태가 구현이 되어있다.

![Spring Web Module](./picture/Spring_Web_Module.JPG)

Spring MVC 기본 동작 흐름은 아래 그림과 같다.

![Spring MVC 기본 동작 흐름](./picture/SpringMVC_기본동작.JPG)

1. 클라이언트가 요청을 보내면, 모든 요청을 Dispatcher Serlvet이 받는다.

2. Dispatcher Servlet은 받은 요청을 처리해줄 컨트롤러와 메서드가 무엇인지 Handler Mapping에게 물어본다.

3. Handler Mapping은 개발자가 등록해둔 Controller관련 xml파일이나 java파일 어노테이션을 통해 알아낸다.

4. Handler Mapping을 통해 알아낸 적절한 Controller를 Dispatcher Servlet이 Handler Adapter에게 실행을 요청한다.

5. Handler Adapter는 매핑된 컨트롤러와 메서드를 실행한다.

6. 그 결과를 Model에 받아서 Dispatcher Servlet에게 전달한다.

7. 이때 Dispatcher Servlet은 컨트롤러가 리턴한 view name을 통해 View Resolver를 통해서 뷰를 출력한다.

이 그림에서 DataBase를 제외한 파란색 부분은 모두 Spring MVC가 제공하는 부분이다.

보라색인 부분들은 개발자가 구현해야하는 부분이다.

녹색은 Spring이 제공하는 부분과 개발자가 구현해야하는 부분 둘 다 존재하는 부분이다.

### **Model**

뷰가 렌더링하는데 필요한 데이터

ex)

사용자가 요청한 상품 목록, 주문 내역

### **View**

웹 어플리케이션에서 실제로 보이는 부분, 모델을 사용해 렌더링을 한다.  

JSP,JSF,PDF,XML등으로 결과를 표현한다.

### **Controller**

사용자의 액션에 응답하는 컴포넌트

모델을 업데이트하고, 다른 액션을 수행한다.

### **MVC Model 1 아키텍처**

![MVC Model 1](./picture/MVC1.JPG)

Model 1은 위와 같은 형태로 JSP Page가 Browser에 응답하므로 
요청만큼 JSP 페이지가 존재해야 한다.  

JSP page는 Java bean(Jdbc를 이용한 RoleDao)을 이용하여 데이터베이스를 사용해 응답한다.

Model 1 아키텍처는 JSP가 HTML과 JAVA 코드로 섞여있어 유지 보수가 어려웠다.

### **MVC Model 2 아키텍처**

![MVC Model 2](./picture/MVC1(2).JPG)

Model 2 아키텍처는 요청은 서블릿이 받게하고 서블릿이 Java bean을 이용하여 DB에서 데이터를 꺼내오고 그 결과를 JSP를 통해 화면에 보여주도록 하는 방법을 사용한다.  

Controller - Servlet  
View - JSP  
Model - Data  

Model 2 아키텍처는 로직과 뷰를 분리할 수 있다.

![MVC Model 발전형태](./picture/Model2발전형태.JPG)

이 그림은 MVC Model2의 발전형태 이다.

클라이언트가 보내는 모든 요청을 프론트 컨트롤러라고 하는 서블릿 클래스가 다 받는다.

전체 서블릿은 이 프론트 컨트롤러 하나이다(이론적으론 하나이상 사용 가능).  
이 프론트 컨트롤러는 요청만 받지 요청의 처리는 핸들러 클래스 혹은 컨트롤러 클래스에게 위임하여 컨트롤러 클래스가 처리하게 한다.

컨트롤러 클래스는 Java bean을 이용하여 요청을 처리하고 그 결과를 모델에다 담고 프론트 컨트롤러에게 보내면 프론트 컨트롤러는 알맞은 뷰에게 모델을 전달해서 그 결과를 출력하게 된다.

### **Spring MVC 구성요소**

- DispatcherServlet
    - HandlerMapping
    - HandlerAdapter
    - MultipartResolver
        - 멀티파트 파일 업로드를 처리하는 전략
    - LocaleResolver
        - 지역 정보를 결정해주는 전략 오브젝트
        - HTTP 헤더의 정보를 보고 지역정보를 설정
    - ThemeResolver
    - HandlerExceptionResolver
    - RequestToViewNameTranslator
    - ViewResolver
    - FlashMapManager
        - FlashMap 객체를 조회&저장을 위한 인터페이스
        - RedirectAttributes의 addFlashAttribute메서드를 이용해서 저장
        - 리다이렉트 후 조회를 하면 바로 정보는 삭제

### **DispatcherServlet 내부 동작 흐름**

### Dispatcher Servlet

Dispatcher Servlet은 모델 2 아키텍처 발전형태의 프론트 컨트롤러라고 한다.

프론트 컨트롤러는 이론적으로는 한 개 이상 사용될 수 있다고 하는데 보통은 한 개만 선언해서 사용한다.

클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.

DispatcherServlet은 위의 여러 컴포넌트를 이용해 작업을 처리한다.

### Dispatcher Servlet 내부 동작

![DispatcherServlet 내부 동작 흐름](./picture/DispatcherServlet.JPG)

Dispatcher Servlet은 요청을 받으면 위와 같은 내부 동작을 거친다.

1. 선처리 작업
2. HandlerExecutionChain 결정 및 실행
3. 예외 발생 시 예외처리
4. 뷰 렌더링
5. 요청 처리 종료

자세한 사항은 아래를 보자.

### 선처리 작업

![선처리 작업](./picture/선처리작업.JPG)

1. Locale 결정
    - 지역화(브라우저 언어 세팅에 따른 페이지 지원)을 해준다.

2. RequestContextHolder 요청 저장
    - RequestContextHolder는 스레드 로컬 객체이다.
    - 요청을 받아서 응답할 때까지 HttpServletRequest, HttpServletResponse등을 Spring이 관리하는 객체 안에서 사용할 수 있도록 해준다.
    - 이를 통해 Controller의 메서드안에서 Request 객체가 필요할 때 매개변수로 HttpServletRequest request 선언해주면 사용할 수 있다.

    ```
    반환형 function(HttpServletRequest request){

    }
    ```
    
    - 하지만 이 방식은 Spring이 웹 기술에 종속되는 문제점을 가지기도 한다.

3. FlashMap 복원
    - Spring 3에서 추가된 기능
    - redirect로 값 전달할 때 ?, 파라미터 이런 것을 사용하면 URL이 굉장히 복잡하다.
    - FlashMap을 사용하면 redirect될 때, 딱 한 번 값을 유지시킬 수 있게 해주는 역할을 한다.

4. MultipartResolver 요청
    - 파일 업로드를 했을 경우에는 파일 정보를 읽어들이는 특수한 형태의 Request 객체가 필요(HttpServletRequest와 다른 Request객체)
    - 이런 멀티 파트 요청이 들어오면, Request를 MultipartResolver가 이런 멀티 파트를 결정하게 해주는 것

5. 실제 요청을 처리하는 핸들러를 결정하고 실행

### 요청 전달 - HandlerExecutionChain 결정

![요청 전달](./picture/요청전달.JPG)

1. HandlerMapping으로 HandlerExecutionChain을 결정

2. HandlerExecutionChain이 없다면 페이지가 없으므로 Http 404 전달, 있다면 HandlerAdapter 결정 

3. HandlerAdapter이 없다면 서버 문제이므로 ServletException 발생, 있다면 요청 처리

### 요청 처리 - HandlerExecutionChain 실행

![요청 처리](./picture/요청처리.JPG)

1. HandlerExecutionChain이 결정되었다면, 사용가능한 인터셉터(일종의 필터)가 존재하는 지 찾는다.

2. 인터셉터가 존재한다면, 인터셉터의 preHandle을 호출해서 요청을 처리하고 핸들러 실행

3. 핸들러가 실행된 후엔 결과값을 리턴하게 되는데 결과가 ModelAndView객체면서, 적절한 뷰가 없을 때 RequestToViewNameTranslator가 동작, 그외에는 인터셉터의 postHandle을 호출해서 요청을 처리하고 뷰 렌더링을 한다.

### 예외 처리

![예외 처리](./picture/예외처리.JPG)


1. HandlerExceptionResolver에서 해당 예외가 있는 지 검색

2. 있다면 예외처리, 없다면 다시 예외를 던짐

### 뷰 렌더링

![뷰 렌더링](./picture/뷰렌더링.JPG)

1. 뷰가 String인 경우, ViewResolver로 View 구현체를 찾음

2. 있다면 View구현체로 렌더링, 없다면 ServletException을 던짐

### 요청 처리 종료

![요청 처리 종료](./picture/요청처리종료.JPG)

1. HandlerExecutionChain이 존재한다면, 인터셉터의 afterCompletion 메서드 실행

2. RequestHanlderEvent 발생

3. 요청 처리 완료