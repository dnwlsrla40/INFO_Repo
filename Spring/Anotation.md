# Spring Anotation

**@AllArgsConstructor**

인스턴스 변수로 선언된 모든 것을 파라미터로 받는 생성자를 작성하게 된다.

```
@Component
@Getter
@AllArgsConstructor
public class SampleHotel{
    private Chef chef;
}
```

따라서, 


**@Bean**

하나의 @Bean 메소드를 통해 얻을 수 있는 빈의 DI 정보는 다음 세 가지다.

- 빈의 이름 : @Bean 메소드 이름이 빈의 이름이다. 이 이름은 getBean()에서 사용된다.
- 빈의 클래스 : 빈 오브젝트를 어떤 클래스를 이용해서 만들지를 정의한다.
- 빈의 의존 오브젝트 : 빈의 생성자나 수정자 메소드를 통해 의존 오브젝트를 넣어준다. 의존 프로젝트도 하나의 빈이므로 이름이 있을 것이고, 그 이름에 해당하는 메소드를 호출해서 의존 오브젝트를 가져온다. 의존 오브젝트는 하나 이상일 수도 있다.

XML에서는 &lt;bean&gt;을 사용한다.