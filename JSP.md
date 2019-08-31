# JSP(Java Server Page)

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