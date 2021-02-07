# JPA(Java Persistence API) 등장 배경

## 순수 JDBC 

```
public class MemberDao {
    public Long save(Connection conn, Member member) {
        PreparedStatement pstmt = null;

        String sql = "INSERT INTO MEMBER(USERNAME, PHONE_NUMBER) VALUES (?, ?);

        try{
            pstmt = conn.conn.prepareStatement(sql);
            pstmt.setString(1, member.getUsername());
            pstmt.setString(2, member.getPhoneNumber());

            pstmt.executeUpdate();

            ResultSetgeneratedKeys = pstmt.getGeneratedKeys();
            if(generatedKeys.next()) {
                long memberId = generaedKeys.getLong(1);
                return memberId;
            }
            return null;
        } catch(Exception e) {
            throw new RuntimeException(e);
        } finally {
            close(pstmt);
        }
    }
}
```

과거에는 객체를 DB에 저장을 하거나 조회를 하려면 위와 같이 복잡한 JDBC API와 SQL문을 직접 작성해야만 했다.

## SQL Mapper

```
public class MEmberDAO {
    private JdbcTemplate jdbcTemplate;
    private SimpleJdbcInsert insertActor;

    public Member findOne(Long id) {
        String sql = "SELECT MEMBER ID, USERNAME, PHONE NUMBER FROM MEMBER WHERE ID = ?"
        return jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Member>(), id);
    }
}
```

후에 이를 간편화 하기 위해서 JdbcTemplate, Mybatis와 같은 SQL Mapper가 등장해서 개발 코드는 줄었지만 위의 코드와 같이 아직 SQL문은 직접 작성해야만 한다.

## JPA

```
public class MemberDAO{
    @PersistenceContext
    EntityManager jpa;

    public void save(Member member) {
        jpa.persist(member);
    }

    public Member findOne(Long id) {
        return jpa.find(Member.class, id);
    }
}

이렇게 JPA를 사용하면 이제는 SQL문 조차도 작성하지 않아도 된다.


이처럼 개발 생산성, 유지보수 측면에서 JPA가 이전과는 확연하게 차이가 나게 된다.