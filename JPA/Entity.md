# Entity

Entity는 Jpa가 관리할 객체로 보통 class에 @Entity라는 어노테이션을 이용하여 등록해준다.


```
@Entity
public class Member {

    @Id
    private Long id;
    private String name;
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

이처럼 관리할 Member table에 관한 내용을 Mapping 할 수 있는 Member 클래스에 @Entity를 붙여주면, Jpa에서 이 Member 객체를 관리해준다.

## 엔티티의 생명주기

- 비영속 (new/transient) : 영속 컨텍스트와 전혀 관계가 없는 새로운 상태
- 영속 (managed) : 영속성 컨텍스트에 관리되는 상태
- 준영속 (detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제 (removed) : 삭제된 상태

![엔티티의 생명주기](/picture/엔티티의_생명주기.PNG)

### 비영속

![비영속 상태](/picture/비영속.PNG)

객체를 생성하기만 한 상태, 세팅만 한 상태  
즉, JPA와는 전혀 관계가 없는 상태를 말한다.


```
// 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

### 영속

1차 캐시에 등록이 되어있는 상태

 - `em.persist(member);`
 - `em.find(member) 해서 없는 경우`

![영속 상태](/picture/영속.PNG)

```
// 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

EntityManager em = emf.createEntityManager();
em.getTransaction.begin();

// 객체를 저장한 상태(영속) = DB에 저장이 되는 것은 아님
em.persist(memeber);
```

실제로 아래와 같이 em.persist 사이에 코드를 넣고 실행해보면 em.persist 이후에 insert문이 실행되는 것을 볼 수 있다.
```
// 비영속
Member member = new Member();
member.setId(100L);
member.setName("HelloNEWJPA");

// 영속
System.out.println("====================BEFORE===================");
em.persist(member);
System.out.println("====================AFTER===================");
```

![DB에 저장 X](/picture/DB에_저장_X.PNG)

그러면 어떻게 해야 DB에 저장이 되는 것일까?

```
tx.commit();
```
을 해야 DB에 저장을한다.

### 준영속

1. `em.detach(member);` : 특정 엔티티만 준영속 상태로 전환

2. `em.clear()` : 영속성 컨텍스트를 완전히 초기화

3. `em.close()` : 영속성 컨텍스트를 종료

를 하면 엔티티를 영속성 컨텍스트에서 분리한 준영속 상태가 된다.

준영속 상태가 되면 영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.

### 삭제

```
em.remove(member);
```
를 하면 객체를 DB에서 삭제를 한다.
