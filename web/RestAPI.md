# RestAPI

**목차**  
[등장](#등장)  
[API](#api)  
[Rest API](#rest-api)  
- [uniform interface](#uniform-interface)  

[결론](#결론)

## **등장**

WEB(1991)

Q : 어떻게 인터넷에서 정보를 공유할 것인가?

A : 정보들을 하이퍼텍스트로 연결한다.

표현 형식 : HTML
식별자 : URI
전송 방법 : HTTP

이렇게 정해진 HTTP가 기존의 Web을 깨트리지 않고 이전의 Web과 호환할 수 있을까?? 혹은 발전할 수 있을까? -> REST 

## **API**

Application Programming Interface의 약자로 응용 프로그램에서 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스

즉, 해당 라이브러리를 사용할 때 구현코드를 알지 못해도 인터페이스를 통해 사용 할 수 있게 해준다.

(리모콘이 어떻게 동작하는 지 모르지만 우리는 TV를 키고 끄고, 채널을 돌릴 수 있다.)

## **Rest API**

REpresentational State Transfer의 약자로 REST 형식의 API를 말한다.

REST는 다음과 같은 스타일을 반드시 지켜야 한다.

- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand(optional)

여기서 말하는 스타일은 위 제약조건의 집합을 의미한다.

Layered System은 
```
http://domain/houses/apartments
```
와 같이 슬래시 구분자(/)를 통한 구분을 말한다.

HTTP프로토콜을 사용하면 나머지는 쉽게 구현 가능하지만, uniform interface의 스타일을 지키면서 구현하기는 힘들다.

### uniform interface

- 리소스가 URI로 식별되야 한다.
- 리소스를 생성,수정,추가하고자 할 때 HTTP메시지에 표현을 해서 전송해야 한다.
- 메시지는 스스로 설명할 수 있어야 한다.(Self-descriptive message)
- 어플리케이션의 상태는 Hyperlink를 이용해 전이되야 한다.(HATEOAS)

메시지가 스스로 설명할 수 있다는 것은 응답 결과인 JSON메시지가 어디에 전달되는지, JSON메시지를 구성하는 것이 어떤 것을 의미하는지 표현되어야 한다는 것을 뜻한다.

HATEOAS는 웹페이지에서 상세보기나 글쓰기로 이동, 글 수정, 글 삭제등으로 갈 수 있는 링크가 있는 것을 볼 수 있는데 이와 같이 웹 페이지 자체와 관련된 링크가 있는 것을 말한다.

## **결론**

이처럼 REST API를 다 지키며 서비스를 운영하는 것은 쉽지않다 따라서 보통은 Web API 혹은 HTTP API를 사용한다.