# Web Scope

Web에서 사용 하는 스코프에는

1. Page Scope
2. Request Scope
3. Session Scope
4. Application Scope

가 존재한다.

## Page Scope

한 페이지 내에서 이뤄지는 일들에 대해서 관여한다.

- pageContext 추상 클래스를 사용한다.
- forward가 될 경우 해당 page scope에 지정된 변수는 사용할 수 없다.
- 마치 지역변수처럼 사용된다.
- jsp에서 page scope에 값을 저장한 후 해당 값을 EL표기법등에서 사용할 때 사용한다.
- 지역 변수처럼 해당 jsp나 서블릿이 실행되는 동안에만 정보를 유지하고자 할 때 사용한다.

해당 요청을 받은 페이지 하나당 pageContext는 하나씩 생긴다.  
이 pageContext는 페이지가 이용될 때까지만 영향을 끼치므로 forward를 통해서 페이지가 넘어갈 경우 해당 pageContext는 메모리에서 없어진다.

### 사용

이름.setAttribute, 이름.getAttribute

## Request Scope

http 요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우 사용한다.

- HttpServletRequest 객체를 사용한다.
- JSP는 request 내장 변수, 서블릿에서는 HttpServletRequest 객체를 사용한다.
- forward시 값을 유지하고자 사용한다.
- 


### 사용

값을 저장할 때 : request(HttpServletRequest) 객체의 setAttribute() 사용  
값을 읽을 때 : request(HttpServletRequest) 객체의 getAttribute() 사용

## Session Scope

웹 브라우저 별로 변수를 관리하고자 할 경우 사용한다.  
즉, 클라이언트 마다 객체를 만들어서 상태를 관리하고자 할 때 사용한다.  

ex) 로그인 정보, 장바구니 정보 등

- 웹 브라우저간의 탭간에는 세션정보가 공유되기 때문에, 각각의 탭에서는 같은 세션정보를 사용할 수 있다.(하나의 탭에서 로그인을 하면 다른 탭에서 연동이 되는 이유)
- JSP는 session 내장 변수 사용, 서블릿에서는 HttpServletRequest의 getSession() 메소드를 이용하여 session 객체를 얻을 수 있다.

### 사용

값을 저장할 때 : session(HttpServletRequest.getSession) 객체의 setAttribute() 사용 
값을 읽을 때 : session(HttpServletRequest.getSession) 객체의 getAttribute() 사용 

## Application Scope

Server에는 Web Application이 여러 개 존재 가능

ex) Application :  예제의 firstWeb 프로젝트 하나

웹 어플리케이션이 시작되고 종료될 때까지 변수를 사용할 수 있다.  
즉 모든 클라이언트가 공통으로 사용해야 할 변수가 필요할 경우 사용한다.

- 웹 어플리케이션 하나당 하나의 application 객체 사용
- JSP는 application 내장 객체 사용, 서블릿은 getServletContext() 메소드를 이용하여 사용
- 서블릿의 ServletContext와 application은 같다.
### 사용

값을 저장할 때 : application 객체의 setAttribute() 사용 
값을 읽을 때 : application 객체의 getAttribute() 사용 


## Scope 사용 시 주의 사항

각 객체의 이름.getAttribute()로 변수를 가져와 후에 가공을 한다고 하더라도 다시 이름.setAttribute()를 하지 않으면 객체 내의 변수는 변하지 않는다.

```
ApplicationScope01.java (Servlet)의 doGet 코드 내용
// value 값을 1로 세팅

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html; charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		
		ServletContext application = getServletContext();
		
		int value = 1;
		application.setAttribute("value",  value);
		
		out.println("<h1>value :" + value +"</h1><br>");
	}
```

```
ApplicationScope02.java (Servlet)의 doGet 코드 내용
// value를 가져와 값 증가

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		ServletContext application = getServletContext();
		try {
			int value = (int)application.getAttribute("value")+1;
			
			out.println("<h1>value : " + value + "</h1><br>");
		}catch(NullPointerException e) {
			out.print("value 값이 설정되지 않았습니다.");
		}
	}
```

위 코드를 실행한 후 ApplicationScope02를 여러번 로드해도 value의 값은 여전히 1이다.