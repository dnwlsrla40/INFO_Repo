# List.jsp 변환

list.jsp는 게시판의 목록으로 content.jsp와 다르게 여러 개의 값을 가져와야 한다.

여러 개의 값을 가져와야하는 request.setAttribute()를 사용하기 애해하다.

따라서 이때 사용하는 것이 Entity Beans라고 하는 디자인 패턴이다

## Entity Beans

DB에서 가져온 데이터들을 담는 그릇이다. 즉, DB에 있는 row(column이 아님) 정보를 Entity Bean 하나에 담는다.

DB에 저장된 데이터를 객체로 표현하기 위한 EJB Component이다.