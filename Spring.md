# Spring

목차  
[들어가기 전](#들어가기-전)  
- [컨테이너](#컨테이너(container))
- [빈](#bean)
  - [빈 등록 방법 - applicationContext.xml에 빈을 등록하는 방법](#빈-등록-방법---applicationcontext.xml에-빈을-등록하는-방법)
  - [빈 등록 방법 - 어노테이션과 Config를 통해 빈을 등록하는 방법](#빈-등록-방법---어노테이션과-config를-통해-빈을-등록하는-방법)

[Spring](#Spring)  
[스프링의 계층](#스프링의-계층)
- [데이터 엑세스(Data Access) / 통합(Integration)](#데이터-엑세스(data-access)-/-통합(integration))
- [웹(Web)](#웹(web))  
  
[IOC(Inversion of Control)](#ioc(inversion-of-control))  
[DI(Dependency Injection)](#di(dependency-injection))
- [Spring에서 제공하는 IoC/DI 컨테이너](#spring에서-제공하는-ioc/di-컨테이너)

[Spring XML](#Spring-XML)  
 - [Web.xml](#webxml)
 - [root-context.xml](#root-contextxml)
 - [servlet-context.xml](#servlet-contextxml)

## **들어가기 전**

Frame Work 
- 개발하기 어려운 부분들에 대해 미리 어느정도 만들어 져있는 제품을 제공하는 것

예를 들어, 우리가 가구를 만들 때 어려움을 겪어서 어느정도 만들어져 있는 반 제품을 사서 나머지를 만드는 것과 비슷하다.


### **컨테이너(Container)**

컨테이너는 인스턴스의 생명주기를 관리한다.

생성된 인스턴스들에게 추가적인 기능을 제공한다.

예를 들면, Servlet을 실행해주는 WAS는 Servlet 컨테이너를 가지고 있다.

### **Bean**

공장이 자동으로 만들어 줄 객체

특징
1. 기본 생성자를 가지고 있다.
2. 필드는 private하게 선언한다.
3. getter, setter 메서드를 가진다.
    - getName(), setName() 메서드가 있다면 name 프로퍼티(property)라고 한다.

### **빈 등록 방법 - applicationContext.xml에 빈을 등록하는 방법** ###

applicationContext.xml 파일을 resource 폴더에 만들어서 등록을 해준다.
```
[applicationContext.xml]
id는 호출될 String이고, class는 연결 시킬 객체의 Class이다.
<bean id="userBean" class="kr.or.connect.diexamo1.UserBean"/>

[다른 곳에서의 사용 - main]
public static void main(String[] args) {
    ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
    System.out.println("초기화 완료!!");
    UserBean userBean = (UserBean)ac.getBean("userBean"); // id = userBean 이다. getBean의 리턴형은 Object이므로 형변환 시켜주어야 한다.
    userBean.setAge(3);
    System.out.println(userBean.getAge());
}
```

Context를 통한 객체 생성은 한번 만 하므로 여러번 객체를 생성하는 것이 아니라 싱글톤 형식으로 동일한 하나의 객체를 만든다.
따라서, 프로그램이 실행이 될때 Context는 applicationContext.xml에 등록된 빈들을 모두 메모리에 등록하고 다른 곳에서 getBean()등을 사용해서 호출하면, 메모리에 등록된 주소를 참조하여 반환한다.

따라서 다른 객체를 만들어 주려면 아래와 같이 xml을 설정해야 한다.

```
[application.xml]
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="c" class="kr.or.connect.diexam01.Car">
		<property name="engine" ref="e"></property>
	</bean>
</beans>

[main]
public static void main(String[] args) {
    Engine e = new Engine();
    Car c = new Car();
    c.setEngine(e);
    c.run(); 
}
```

이처럼 프로퍼티(setter or getter)를 통하여 다른 객체로 등록이 가능하다 setEngine은 파라미터로 (Engine e)를 가지기 때문에 `ref="e"` 속성을 등록하였다.

이러한 방식으로 등록을 하면 main을 위와 같은 형태가 아닌
```
public static void main(String[] args) {
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    Car car = (Car)ac.getBean("c");
    car.run();
}
```
이와 같이 작성가능하다.

Engine 객체와 Car객체를 생성해주지 않아도 car.run();을 실행 시킬 수 있다.

![IOC를 통한 객체 주입](./picture/IOC.JPG)

### **빈 등록 방법 - 어노테이션과 Config를 통해 빈을 등록하는 방법** ###

applicationContext.xml 방식은 새로운 객체가 나올 때마다 등록해주어야하고, ApplicationContext 팩토리(객체) 역시 생성해줘야 하므로 번거롭다.   
그래서 최근에는 어노테이션과 JAVA Config를 통한 방법을 선호한다.

각각에 필요한 Config Class를 생성한다 여기서는 ApplicationConfig를 생성해준다. 그 후 이 클래스가 Config파일임을 알려주기 위해 @Configuration 어노테이션을 사용한다.

```
@Configuration
public class ApplicationConfig {

}
```

그 다음으론, Bean을 등록해주어야 한다.

```
@Configuration
public class ApplicationConfig {
    @Bean
    public Car car(Engine e){
        Car c = new Car();
        c.setEngine(e);
        return c;
    }

    @Bean
	public Engine engine() {
		return new Engine();
	}
}
```

이와 같이 등록하였다면 

```
[main]
public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
    
    Car car = (Car)ac.getBean(Car.class);
    car.run();
}
```
처럼 사용할 수 있다.
하지만 위와 같은 코드는 applicationContext.xml보다 편리하지 않아 보인다. 좀 더 다양한 어노테이션을 이용해 더 간결하게 바꿀 수 있다.

@ComponentScan 어노테이션은 컨테이너에게 해당 패키지 내의 어노테이션들을 읽어 빈으로 등록하는 일을 한다.

@ComponentScan 어노테이션이 읽는 어노테이션

1. 컨트롤러
2. 서비스
3. 레파지토리
4. 컴포넌트
5. etc...

```[ApplicationConfig.java]
@Configuration
@ComponentScan("kr.or.connect.diexam01")
public class ApplicationConfig02 {

}
```
이렇게 등록을 해주면 컨테이너가 "kr.or.connect.diexam01" 패키지 내의 클래스를 돌며 컨트롤러,서비스,레파지토리,컴포넌트 등의 어노테이션을 읽어 빈으로 등록한다.

그러면 Car와 Engine 클래스도 아래 처럼 간편해진다.

```
[Engine.java]
@Component  //Component 어노테이션
public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}

[Car.java]
@Component
public class Car {
	@Autowired
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
//	public void setEngine(Engine e) {
//		this.v8 = e;
//	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
```
Car와 Engine을 읽을 수 있도록 @Component 어노테이션을 붙이고, Setter와 Getter 메서드를 통한 객체 생성이 필요없어졌다.



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

## **IOC(Inversion Of Control)**

개발자가 프로그램의 흐름을 제어하는 코드를 작성하는데 이 흐름의 제어를 개발자가 하는 것이 아니라 다른 프로그램이 그 흐름을 제어하는 것을 IOC라 한다.

즉, 개발자가 만든 어떤 클래스나 메소드를 다른 프로그램이 대신 실행해주는 것을 말한다.


예를 들어, TV(인터페이스)에 관한 객체로 S_TV와 L_TV가 있다고 한다면,

우리는
```
Tv tv = new Class();
tv.메소드;
```
이러한 방식으로 사용할 것이다.

하지만, 이렇게 사용을 하면, S_TV와 L_TV에 따라서 Class나 메소드를 변경해줘야 하는 번거로움이 있다.

그래서, 이러한 부분을 대신 해주는 TVFactory 라는 것이 있다면

```
Tv tv = TvFactory.getTv("클래스");
tv.메소드;
```
방식으로 특정한 TVFactory가 uri 패턴이나 정보로 상황에 따라 getTv의 매개변수로 S_Tv나 L_Tv를 넣어줘 객체를 생성해주는 것이다.

여기서 TvFactory와 같은 프로그램의 역할은 Spring의 Bean Factory나 Application Context 같은 것이 해준다. 

## **DI(Dependency Injection)**

위의 IOC에서 공장으로 인스턴스를 만들었다면 사용을 하기 위해 이 객체를 받아와야한다.

DI는 이 객체를 받아오는 방법 중 하나로 의존성 주입이라고도 한다.

DI는 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.

예를 들어, DI를 사용하지 않으면 개발자가 아래처럼 직접 인스턴스를 생성해야만 한다.

```
//DI 사용하지 않을 때
Class 엔진{

}

Class 자동차{
    엔진 v5 = new 엔진();
}
```
![의존성 주입 X](./picture/DI.JPG)

하지만, Spring에서 제공하는 DI를 사용하면 어노테이션을 이용하여 IOC/DI 컨테이너가 객체를 생성하도록 한다.

즉, 객체를 대신 메모리에 올려준다.

```
//Spring에서 DI를 사용할 때

@Component
Class 엔진{

}

@Component
Class 자동차{
    @Autowired
    엔진 v5;
}
```  

![의존성 주입 O](./picture/DI사용.JPG)

### **Spring에서 제공하는 IoC/DI 컨테이너**

- BeanFactory
  - IoC/DI에 대한 기본 기능을 가지고 있다.
  - 기본적인 Factory

- ApplicationContext 
  - BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천된다.
  - 트랜잭션처리, AOP등에 대한 처리를 할 수 있다. BeanPostProcessor, BeanFactoryPostProcessor등을 자동으로 등록하고, 국제화 처리, 어플리케이션 이벤트 등을 처리할 수 있다.

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