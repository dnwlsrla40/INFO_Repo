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