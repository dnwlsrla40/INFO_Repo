# Spring

목차  
[1. Spring XML](#Spring-XML) 
 - [Web.xml](#webxml)
 - [root-context.xml](#root-contextxml)
 - [servlet-context.xml](#servlet-contextxml)

## Spring XML

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