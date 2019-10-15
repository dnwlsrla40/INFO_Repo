# DAO(Data Access Object)

**목차**

[DAO](#dao)   

## **DAO**

DAO는 데이터를 조회하거나 조작하는 기능을 하도록 만든 객체이다.

보통 데이터베이스를 조작하는 기능을 위한 목적으로 만들어 진다.

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