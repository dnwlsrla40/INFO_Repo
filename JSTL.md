# JSTL(JSP Standard Tag Library)

**목차**  

[1.JSTL](#JSTL)  
[2.사용 이유](#사용-이유)  
[3.사용 방법](#사용-방법)  
[4.태그](#태그)
- [코어](#코어)  
- [XML](#XML)  
- [국제화](#국제화)
- [데이터베이스](#데이터베이스)
- [함수](#함수)  

## JSTL 

JSTL은 JSP 페이지에서 조건문 처리, 반복문 처리 등을 html tag의 형태로 작성할 수 있게 도와준다.

## 사용 이유

기존의 JSP는 개발의 편의성은 높았지만 HTML과 JAVA 코드가 섞여있어 프론트 개발자가 해당 코드를 수정하기가 힘들었다.  
즉 유지보수가 어려웠다.

따라서, JSTL을 사용하여 Java 코드를 없애고 프론트 엔드 개발자가 유지보수를 쉽게 할 수 있게 되었다.

## 사용방법

[jar 파일 다운로드](http://tomcat.apache.org/download-taglibs.cgi) 사이트에 가서   

- Impl:
  - taglibs-standard-impl-(version)
- Spec:
  - taglibs-standard-spec-(version)
- EL:
  - taglibs-standard-impl-(version)

3가지 jar 파일을 다운로드 한 후 WEB-INF/lib 폴더에 복사를 한다.

그 후 사용할 jsp 파일에서 
`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core%>`를 지정해서 각 키워드를 사용할 수 있다.

## 태그

### 코어

변수지원, 흐름 제어 URL 처리등의 기능을 한다.

- 접두어 : c
- URI : http://java.sun.com/jsp/jstl/core

ex) 변수 사용(set) 및 제거(remove)

1. 일반 변수

```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:set var="value1" scope="request" value="kang"/>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
성 : ${value1 }<br>
<c:remove var="value1" scope="request"/>
</body>
</html>
```

2. 객체 프로퍼티 혹은 맵

```

<c:set target="${some(객체 이름)}" property="propertyName(프로퍼티 이름)" value="anyValue(변수 이름)" />
```

some 객체가 자바빈일 경우: some.setPropertyName(anyvalue)  
some 객체가 맵(map)일 경우 : some.put(propertyName,anyvalue);





### XML

XML 코어, 흐름제어, XML 변환 등의 기능을 한다.

- 접두어 : x
- URI : http://java.sun.com/jsp/jstl/xml

### 국제화

지역, 메시지 형식, 숫자 및 날짜 형식 등의 기능을 한다.

- 접두어 : fmt
- URI : http://java.sun.com/jsp/jstl/fmt

### 데이터베이스

SQL과 관련된 기능을 한다.

- 접두어 : sql
- URI : http://java.sun.com/jsp/jstl/sql

### 함수

콜렉션 처리, String 처리등의 기능을 한다.

- 접두어 : fn
- URI : http://java.sun.com/jsp/jstl/functions