# 서블릿(Servlet)이란?

## 자바 웹 어플리케이션

- WAS(ex.tomcat)에 설치되어 동작하는 어플리케이션
- 자바 웹 어플리케이션에서는 HTML. CSS, 이미지, 자바로 작성된 클래스(Servlet, package, interface), 각종 설정 파일 등이 포함
- servlet 3.0 미만에서는 web.xml 파일 필수 3.0이상부턴 어노테이션 사용

## Servlet

자바 웹 어플리케이션의 구성요소 중 동적인 처리를 하는 프로그램의 역할

- WAS에서 동작하는 JAVA 클래스
- HttpServlet 클래스를 상속받아야한다.

## 작성방법

1. Servlet 3.0 이상에서 사용방법
   - Java annontation 어노테이션 사용

```
@WebServlet("/ten")
public class TenServlet extends HttpServlet {

}
```

2. Servlet 3.0 미만에서 사용방법
   - web.xml 파일에 등록

```
<servlet>
    <description>/description>
    <display-name>TenServlet</display-name>
    <servlet-name>TenServlet</servlet-name>
    <servlet-class>exam.TenServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>TenServlet</servlet-name>
    <url-pattern>/ten</url-pattern>
</servlet-mapping>
```

## 서블릿 생명주기

![서블릿 생명주기](/picture/Servlet생명주기.JPG)

1. WAS는 서블릿 요청을 받으면 해당 서블릿이 메모리에 있는지 확인
2. 없으면 해당 서블릿을 메모리에 올리고(생성자) init() 메소드를 실행
3. 그후 service() 메소드를 실행 -> 즉 init() 후 새로고침 해도 service()메소드만 실행한다.
4. WAS가 종료되거나 웹 어플리케이션이 변경되면 destroy() 메소드 실행