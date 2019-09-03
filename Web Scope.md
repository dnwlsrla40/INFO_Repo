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