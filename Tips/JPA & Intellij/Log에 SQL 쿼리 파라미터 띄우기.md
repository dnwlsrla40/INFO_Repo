# Test 개발 시 Log에 SQL 쿼리 파라미터 찍기

## 기본 세팅

application.yml의 `org.hibernate.SQL: debug`에 의해 insert 쿼리는 보이지만 파라미터는 보이지 않음

![기본 세팅](/picture/No_parameter_information.PNG)


## application.yml에 org.hibernate.type: trace 추가

```
application.yml

logging:
  level:
    org.hibernate.type: trace # 쿼리의 파라미터(?의 내용)을 log에 출력
```

![org.hibernate.type 적용 후](/picture/org.hibernate.type.PNG)

## github의 p6spy 라이브러리 추가

[라이브러리 github 링크](https://github.com/gavlyukovskiy/spring-boot-data-source-decorator)

```
build.gradle

implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.6'
```

![p6spy 적용 후](/picture/p6spy.PNG)