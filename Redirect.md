# 리다이렉트(Redirect)

## Redirect 란?

서버가 클라이언트로부터 요청을 받은 후, 클라이언트에게 특정 URL로 이동하라고 요청하는 것

http프로토콜 규칙

## 과정

1. 서버에서는 클라이언트에게 응답으로 상태코드 302와 이동할 URL정보를 Location 헤더(Header)에 담아 전송
2. 클라이언트는 서버로부터 받은 상태가 302면 Location헤더값으로 재요청
3. 재요청할 때 브라우저의 주소창은 전송받은 URL로 바뀌게 된다.

![Redirect 과정](/picture/Redirect_과정.JPG)

## 적용

서블릿이나 JSP는 redirect를 하기 위해 HttpServletResponse 객체의 sendRedirect()메소드를 사용

### 예시 

redirect01.jsp를 실행시키면 redirect02.jsp로  redirect시키고 redirect02.jsp에선 redirect되었다는 문구 브라우저에 출력

```
redirect01.jsp
<%
	response.sendRedirect("redirect02.jsp");
%>
```

```
redirect02.jsp
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	redirect된 페이지 입니다.
</body>
</html>
```

### 결과

![Redirect 결과](/picture/Redirect_결과.JPG)

결과는 redirect02.jsp의 내용이 나온 것을 볼 수 있다. 하지만 URL이 redirect02.jsp로 되어있는 것으로 redirect가 재대로 수행이 되었단 걸 알 수 있다.

![Redirect Network](/picture/Redirect_Network.JPG)

또한, 위 그림처럼 Network탭을 통하여 redirect01.jsp 요청을 수행후에 코드에 따라 redirect02.jsp로 이동한 것을 볼 수 있다.