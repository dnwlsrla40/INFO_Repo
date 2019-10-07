# Spring

목차  
[들어가기 전](#들어가기-전)  
[Spring](#Spring)  
[스프링의 계층](#스프링의-계층)
- [데이터 엑세스(Data Access) / 통합(Integration)](#데이터-엑세스(data-access)-/-통합(integration))
- [웹(Web)](#웹(web))
[Spring XML](#Spring-XML)  
 - [Web.xml](#webxml)
 - [root-context.xml](#root-contextxml)
 - [servlet-context.xml](#servlet-contextxml)

## **들어가기 전**

Frame Work 
- 개발하기 어려운 부분들에 대해 미리 어느정도 만들어 져있는 제품을 제공하는 것

예를 들어, 우리가 가구를 만들 때 어려움을 겪어서 어느정도 만들어져 있는 반 제품을 사서 나머지를 만드는 것과 비슷하다.

## **Spring**

- 프레임 워크중 하나로 엔터프라이즈급(규모가 큰) 어플리케이션을 구축할 수 있는 가벼운 솔루션이다.
- 전체를 사용하지 않고 원하는 부분만 가져다 사용할 수 있도록 모듈화가 잘 되어 있다.
- IoC 컨테이너다
- 선언적으로 트랜잭션을 관리할 수 있다.
- MVC Framework를 제공한다.
- AOP 지원한다.
- 도메인 논리 코드와 쉽게 분리될 수 있는 구조를 가지고 있다.

## **스프링의 계층**

### **데이터 엑세스(Data Access) / 통합(Integration)

스프링의 데이터 엑세스/통합 계층은 JDBC, ORM, OXM, JMS 및 트랜잭션 모듈로 구성되어 있다.

- spring-jdbc : 자바 JDBC프로그래밍을 쉽게 할 수 있도록 기능을 제공한다.

### **웹(Web)**

스프링의 웹 계층은 spring-web, spring-webmvc, spring-websocket, spring-webmvc-portlet 모듈로 구성되어 있다.

- spring-web : 멀티 파트 파일 업로드, 서블릿 리스너 등 웹 지향 통합 기능을 제고, HTTP클라이언트와 Spring의 원격 지원을 위한 웹 관련 부분 제공
- spring-webmvc : Web-Servlet 모듈이라고 불리며, Spring MVC 및 REST 웹 서비스 구현을 포함한다.

## **Spring XML**

spring 프로젝트 구동 시 관여하는 XML은 web.xml, root-context.xml, servlet-context.xml 파일이 있다.

### Web.xml

Tomcat 구동과 관련된 설정

프로젝트 실행시 가장 먼저 구동되는 Context Listener가 등록되어 있다.

```
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/spring/root-context.xml<param-value>
</context-param>
```

이처럼 context-param에는 root-context.xml의 경로를 설정

### root-context.xml

### servlet-context.xml

스프링 MVC의 특정한 객체(빈)을 설정