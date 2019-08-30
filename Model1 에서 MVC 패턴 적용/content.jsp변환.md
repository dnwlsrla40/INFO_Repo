# List.jsp 변환

## 변환 코드

```
<%@ page import="java.util.regex.Pattern"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<%@ page import="java.sql.*" %>

/* 
    HTML 관련 내용
 */

</head>

<%

try{
    /*
        DB 설정 내용
    */
	
	while(rs.next()){
	
%>
<body>
	<h1>게시글 조회</h1>
	<table border ="1">
		<tr>
			<th>번호</th>
			<td><%=rs.getString("idx") %></td>
			<th>작성자</th>
			<td><%=rs.getString("writer") %> </td>
			<th>날짜</th> 
			<td><%=rs.getString("regdate") %></td>
			<th>조회수</th>
			<td><%=rs.getString("count") %></td>
		</tr>
		<tr>
			<th colspan="2">제목</th>
			<td colspan="6"><%=rs.getString("title") %></td>
		</tr>
		<tr>
			<th colspan="2">내용</th>
			<td colspan="6"><%=rs.getString("content") %></td>
		</tr>
	</table>
	<a href = "delete.jsp?idx=<%=rs.getString("idx") %>">게시글 삭제</a>
	<a href = "list.jsp">목록으로 </a>

<%	
	/*
        DB 해제 및 Error 처리
    */
%>
</body>
</html>
```

이와 같이 코드 상에 여러 곳의 <% %>를 이용하여 jsp에 DB 내용을 가져와 사용하고 있는 것을 볼 수 있다 또한, 변수 사용에 <%=%>를 사용하고 있는 것을 볼 수 있다.

이를 MVC 패턴으로 변환하면,

```

<%@ page import="java.util.regex.Pattern"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
	pageEncoding="EUC-KR"%>
<%@ page import="java.sql.*" %>

// jstl 사용
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

/* 
    HTML 관련 내용
 */

</head>

<%

try{

    /*
        DB 설정 내용
    */
	
	while(rs.next()){
		request.setAttribute("idx", rs.getString("idx"));
		request.setAttribute("writer", rs.getString("writer"));
		request.setAttribute("regdate", rs.getString("regdate"));
		request.setAttribute("count", rs.getString("count"));
		request.setAttribute("title", rs.getString("title"));
		request.setAttribute("content", rs.getString("content"));
	}
	con.close();
}catch(Exception e){
	out.println("Oracle Database Connection Problem <hr>");
	out.println(e.getMessage());
	e.printStackTrace();
}
%>
<body>
	<h1>게시글 조회</h1>
	<table border ="1">
		<tr>
			<th>번호</th>
			<td>${idx }</td>
			<th>작성자</th>
			<td>${writer }</td>
			<th>날짜</th> 
			<td>${regdate }</td>
			<th>조회수</th>
			<td>${count }</td>
		</tr>
		<tr>
			<th colspan="2">제목</th>
			<td colspan="6">${title }</td>
		</tr>
		<tr>
			<th colspan="2">내용</th>
			<td colspan="6">${content }</td>
		</tr>
	</table>
	<a href = "delete.jsp?idx=${idx }">게시글 삭제</a>
	<a href = "list.jsp">목록으로 </a>

</body>
</html>
```

이와 같이 <% %> 스크립트릿을 한 곳으로 모아 구현하여 스크립트릿과 자바스크립트, HTML이 구분되어 있습니다. 

또한, 변수사용에 `${}`를 이용하고 있는 것을 볼 수 있습니다.