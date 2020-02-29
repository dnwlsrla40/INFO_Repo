# Spring MVC

**목차**  
[Spring MVC](#spring-mvc)
- [Model](#model)
- [View](#view)
- [Controller](#controller)
- [MVC Model 1 아키텍처](#mvc-model-1-아키텍처)
- [MVC Model 2 아키텍처](#mvc-model-2-아키텍처)
- [Spring MVC 구성요소](#spring-mvc-구성요소)
- [DispatcherServlet 내부 동작 흐름](#dispatcherservlet-내부-동작-흐름)

## **Spring Application Context**

기존에 오브젝트 팩토리(daoFactory)에 대응되는 것이 스프링의 Application Context이다.

```
<daoFactory>

public class DaoFactory {
	public UserDao userDao() {
		return new UserDao(connectionMaker()); 
	}
	
	public ConnectionMaker connectionMaker() {
		return new DConnectionMaker();
	}
}

Dao 클래스들에 대한 설정 정보(관계, 생성 등) 기존의 능동적인(생성 -> 필요할 때 기능 이용(.userDao())이러한 방식이 아닌 (필요할 때 -> 생성)의존성 역전을 이루는 방식 즉, 필요할 때 UserDao나, ConnectionMaker를 호출하여 db 사용과 연결을 관리한다.
```

이 애플리케이션 컨텍스트를 **IoC 컨테이너**라 하기도 하고, 간단히 **스프링 컨테이너**라고 부르기도 한다. 스프링의 가장 대표적인 오브젝트이므로 그냥 스프링이라고 부르는 개발자도 있다.

## **DaoFactory와 ApplicationContext 차이**

```
직접 생성한 DaoFactory 오브젝트 출력 코드

DaoFacotry factory = new DaoFactory();
UserDao dao1 = factory.userDao();
UserDao dao2 = factory.userDao();

System.out.println(dao1);
System.out.println(dao1);
```

를 하면 두 오브젝트는 동등성은 유효하지만 동일성은 유효하지 않다. 즉 두 오브젝트는 동일한 정보를 가지곤 있지만, 똑같은 오브젝트는 아니다.
하지만 ApplicationContext에 등록해서 getBean()으로 오브젝트를 생성한다면, 생성된 두 오브젝트는 똑같은 오브젝트를 참조한다.

```
스프링 컨텍스트로부터 가져온 오브젝트 출력 코드

ApplicationContext context = new AnnotationConfigApplicationContext(DaoFactory.class);

UserDao dao3 = context.getBean("userDao", UserDao.class);
UserDao dao4 = context.getBean("userDao", UserDao.class);

System.out.println(dao3);
System.out.println(dao4);
```

즉, 스프링은 여러 번에 걸쳐 빈을 요청하더라도 매번 동일한 오브젝트를 돌려준다.