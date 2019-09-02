# Spring MVC 패턴 변환 과정

목차  
[과정](#과정) 
[사용법](#사용법) 

## 과정

MVC에서는 Model1의 jsp 파일 즉, <% %> 구문과 아닌 구문들이 섞여있는 파일을 구별한다.

MVC에서는 스크립트릿(<% %>구문)을 src폴더 안의 자바코드로 다룬다.
그렇게 되면 .jsp에는 HTML코드와 JS코드만 남게 된다.

그러면 scr폴더에 있는 스크립트릿이 어떻게 .jsp에서 사용되는가??

## 사용법

JSP에서는 EL(Expression Language)라고하는 표현언어를 통해서 사용할 수 있게 해준다.

`jstl.jar`파일을 다운 받아서 WebContent->WEB-INF->lib 폴더에 넣으면 된다.

그 후에는 라이브러리를 사용하려는 파일에

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

를 선언해준다.

이로써 사용 준비는 다 끝났고 나머지는 각 코드의 <%=%> 부분 대신 ${변수 이름}을 사용해주면 된다.

## 과정 1 (List 요청 시)

### 흐름도

list.do 요청 -> web.xml에서 url 파악후 ControllerAction 전달 -> ControllerAction에서 url 분석 후 list 관련이니  ListAction으로 전달 -> ListAction에서 Model로 게시판 전체목록 쿼리 실행하기 위해 BoardDao 실행 -> BoardDao에서 DB 접속을 위해 CommonDao(공통 DB 접속, 종료 객체) 실행 -> 받아온 결과 값으로 view로 반환(.jsp)