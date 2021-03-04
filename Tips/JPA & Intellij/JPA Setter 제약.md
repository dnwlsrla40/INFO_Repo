# JPA 개발 시 Setter 제약

JPA 개발 시에

```
OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);
```
와 같이 생성자를 통한 초기화 혹은 생성 매서드를 통한 초기화를 해주어 데이터의 변경 가능성을 최소화 시켜야 한다.

하지만 개발 시 다른 개발자는 

```
Order order = new Order();
order.setItem(item);
```
과 같은 방식으로 개발 스타일을 가져갈 수 있다. 이는 후에 유지보수에 매우 좋지 않다. 따라서 이에 제약을 거는 것을 추천한다.

1. protected 기본 생성자 생성
```
// Domain/OrderItem

public class OrderItem{
    protected OrderItem() {

    }
}
```
JPA에선 protected 접근자 까지 허용하므로 기본 생성자를 protected로 생성해준다.

2. Lombok @NoArgsConstructor(access = AccessLever.PROTECTED)

```
// Domain/OrderItem

@NoArgsConstructor(access = AccessLever.PROTECTED)
public class OrderItem{
}
```

Lombok을 사용한다면 위의 어노테이션 한 줄로 1.과 같은 효과를 누릴 수 있다.