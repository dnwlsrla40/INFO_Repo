# DTO(Data Transfer Object)

**목차**

[1.EL](#EL)  
[2.기능](#기능) 

## **DTO**

DTO는 계층간 데이터 교환을 위한 Java Beans이다.  
여기서 계층이란 컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계층을 의미한다.

일반적으로 DTO는 로직을 가지고 있지 않고, 순수한 데이터 객체이다.

필드와 getter, setter를 가지고, 추가적으로 toString(), equals(), hashCode()등의 Object 메서드를 오버라이딩 할 수 있다.

```
public class ActorDTO{
     private Long id;
    private String firstName;
    private String lastName;
    public String getFirstName() {
        return this.firstName;
    }
    public String getLastName() {
        return this.lastName;
    }
    public Long getId() {
        return this.id;
    }
}
```