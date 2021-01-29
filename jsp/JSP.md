# JSP(Java Server Page)

**목차**  

[JSP란?](#jsp란?)  
[사용 기술(지시자)](#사용-기술(지시자))   
[JSP 파일이 실행될 때](#jsp-파일이-실행될-때)  
[Servlet과 JSP 연동](#servlet과-jsp-연동)  

## JSP란?

쉽게 웹을 개발할 수 있는 스크립트(script) 엔진

서블릿 기술들을 사용한다.
-> 모든 JSP는 서블릿으로 바뀌어서 동작한다.

이론적으로 자바외에 다른 언어들을 사용가능하다. 

## 사용 기술 (지시자)

서블릿으로 바뀌는 부분들을 컴퓨터에게 알려주는 기술 태그(기호)들

### `<%@ %>`

```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
```

1. page 지시자로 자바를 사용할 것이란 것을 알려준다.
2. 응답 출력 결과가 text/html파일이고 utf-8을 사용한다.
3. 이 파일이 UTF-8로 인코딩되어있다.

### `<% %>`

프로그래밍 코드 기술에 사용한다.
자바코드를 적을 수 있다.  
스크립트릿이라고 한다.

```
<%
    int total = 0;
    for(int i = 0; i<=10; i++){
        total += 1;
    }
%>
```

```
<%
	for(int i=1; i<=5; i++){
%>
	<h<%= i%>>안녕</h<%= i%>>
<%
	}
%>
```

### `<%=%>`

값이 들어간 변수를 그대로 출력할 수 있다.  
화면에 출력할 내용 기술에 사용한다.  
서블릿의 out.print()로 변환된다.   
표현식이라 한다.

```
<%= total %>
```

`<%@ %>`, `<% %>`태그는 JSP페이지가 Web 서버를 통해 Load될 때, 가장 먼저 구동된다.

### `<%! %>`

선언식이라고 한다.  
_jspService() 코드의 밖에 즉, 이 클래스(파일명)에 메서드를 선언하던가 필드를 선언할 때 사용한다.

주로 JSP페이지 내에서 필요한 멤버변수나 메소드가 필요할 때 선언하여 사용한다.

```
<%!
    public void jspInit(){
        System.out.print("jspInit()");
    }
    public void jspDestory(){
        System.out.print("jspDestroy()");
    }
%>
```

```
id : <%= getId() %>
<%! 
    String id = "u001"; // 멤버 변수 선언
    public String getId(){  // 메서드 선언
        return id;
    }
%>
```

## JSP 파일이 실행될 때

- 이클립스 워크스페이스 아래의 .metadata 폴더에 "파일명_jsp.java" 파일이 생성된다. (최초 실행에만)
- 해당 파일의 jsp 내용이 서블릿으로 변환되어서 들어가 있다 (Servlet 생명주기에 맞게 _jspService() 메소드 안에 들어가있다)
- 이 "_jsp.java" 파일은 서블릿 소스로 자동으로 컴파일 되면서 실행되어 결과가 브라우저에 보여진다.


1. 브라우저가 웹서버에 JSP 요청
2. JSP 최초 요청시 JSP 엔진에 의해 JSP 코드가 서블릿 코드로 변환(java 파일 생성)
3. 서블릿 코드를 컴파일 해서 실행가능한 bytecode로 변환(class 파일 생성)
4. 서블릿이 실행되어 요청에 대한 응답 정보를 생성

## Servlet과 JSP 연동

[Servlet](https://github.com/dnwlsrla40/INFO_Repo/blob/master/Servlet.md)은 자바 파일이므로 자바 코드를 사용하여 프로그램 로직을 구현하는데 용이하다 그에 반해 JSP는 자바 코드를 쓰기 위해선 스크립트릿(`<% %>`)이나 선언문(`<%! %>`)을 사용해야 한다.

반대로, Servlet은 HTML 페이지를 구현하려면 `out.print()`에서 문자열로 html태그 들을 넣어서 출력하여야 한다. 그에 반해 서블릿은 HTML과 Java 코드를 분리하여 기존의 HTML 코드를 이용하면 쉽게 구현할 수 있다.

따라서, 이런 Servlet과 JSP의 장단점을 해결하기 위해 Servlet에서는 프로그램 로직을 수행하게 하고 그 결과를 JSP에서 출력하는 방식이 좋다.

즉, Servlet에서 프로그램 로직을 수행하고 그 결과를 Request 객체에 넣어 JSP로 포워딩을 하는 것이 좋다.

이것을 JSP와 Servlet 연동이라고 한다.

하지만 JSP에서 HTML 코드를 출력할 때도 Servlet에서 받아온 값을 가공할 때 JAVA 코드를 사용하게 되어 불가피하게 스크립트릿을 사용하게 된다. 또한, HTML 코드 사이사이에 JSP 코드가 섞여 있는 것은 코드의 가독성을 떨어트리게 되어 후에 EL(표현언어)라던가 jstl을 이용하여 표현을 간편하게 하게 된다.

```
JSP 스크립트릿 코드

<%
	int sum = (Integer)request.getAttribute("sum");
	int num1 = (Integer)request.getAttribute("num1");
	int num2 = (Integer)request.getAttribute("num2");
%>
<%=num1 %> + <%=num2 %> = <%= sum %>

EL jstl 코드

${num1} + ${num2} = ${sum}
```