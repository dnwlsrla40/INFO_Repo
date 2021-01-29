# 표현언어(Expression Language)이란?

**목차**

[1.EL](#EL)  
[2.기능](#기능)  
[3. 사용 방법](#사용-방법)  
[4. 기본 객체](#기본-객체) 
[5. 표현 언어 비활성화](#표현-언어-비활성화) 

## EL

값을 표현하는 데 사용되는 스크립트 언어  
JSP의 기본 문법을 보완하는 역할을 한다. (자바 코드와 HTML 코드의 집중성(분리)을 위해 즉, 효율적으로 나눠서 사용하기 위해 사용)  
디자이너 혹은 Front-End만 해본 개발자들이 JSP 파일을 봤을 때 이해하기 쉽도록 최소한의 프로그램 로직을 나타낼 수 있도록 설계되었다.

## 기능

- JSP의 Scope에 맞는 속성 사용
- 집합 객체에 대한 접근 방법 제공
- 수치 연산, 관계 연산, 논리 연산자 제공
- 자바 클래스 메소드 호출 기능 제공
- 표현언어만의 기본 객체 제공

## 사용 방법

`${expr}` 을 이용한다.  

expr - 표현언어가 정의한 문법에 따라 값을 표현하는 식 

```
<jsp:include page="/module/${skin.id}/header.jsp" flush="true"/>
<b>${sessionScope.member.id}</b>님 환영합니다.
```

위처럼 EL은 텍스트 출력부분, HTML 코드 사이등 원하는 여러 위치에 편하게 사용 가능하다.

`${<표현1>.<표현2>}`

- 표현 1이나 표현 2가 null이면 null 반환
- 표현 1이 Map이면 표현 2는 key값 반환
- 표현 1이 List나 배열이면 표현 2가 정수일 경우 정수번 째 index에 해당하는 값 반환
- 표현 1이 객체일 경우 표현 2는 해당하는 getter메소드의 결과 반환

## 기본 객체

### pageContext

JSP의 page 기본 객체와 동일

### pageScope

pageConext 기본 객체에 저장된 속성의 <속성, 값> 매핑을 저장한 Map 객체

### requestScope

request 기본 객체에 저장된 속성의 <속성, 값> 매핑을 저장한 Map 객체

### sessionScope

session 기본 객체에 저장된 속성의 <속성, 값> 매핑을 저장한 Map 객체

### applicationScope

application 기본 객체에 저장된 속성의 <속성, 값> 매핑을 저장한 Map 객체

### param

요청 파라미터의 <파라미터이름, 값> 매핑을 저장한 Map 객체  
파라미터 값은 String 타입, request.getParameter(이름) 결과와 동일

### paramValues

param과 동일하나 결과와 매핑이 배열로 되어 있다.

### header

요청 정보의 <헤더이름, 값> 매핑을 저장한 Map 객체  
request.getHeader(이름) 결과와 동일

### headerValues

header와 동일하나 결과와 매핑이 배열로 되어 있다.

### cookie

<쿠키 이름, Cookie> 매핑을 저장한 Map 객체  
request.getCookies()로 구한 Cookie 배열로부터 매핑을 생성 

### initParam

초기화 파라미터의 <이름, 값> 매핑을 저장한 Map 객체  
application.getInitParameter(이름)의 결과와 동일

```
HTML 파일

<%@ page contentType = "text/html; charset=UTF-8 %>

<% 
    request.setAttribute("name","홍길동");
%>
<html>
<head><title></title></head>
<body>

요청 URI: ${pageContext.request.requestURI}<br>
request의 name 속성: ${requestScope.name}<br>
code 파라미터: ${param.code}
</body>
</html>
```

## 표현 언어 비활성화

JSP에 아래 코드 명시

```
<%@ page isELIgnored = "true" %>
```