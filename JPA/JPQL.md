# JPQL(객체 지향 쿼리 언어)

JPA는 SQL을 추상화한 객체 지향 쿼리 언어인 JPQL을 제공한다.

SQL과 문법이 유사하지만 큰 차이점으로 데이터베이스 테이블 대상이 아닌 JPA가 관리하는 엔티티 객체를 대상으로 쿼리를 날린다는 것이다.

즉, 언어(Mysql, Oracle, etc...)에 종속적이지 않다. -> 페이징 처리 등에 매우 유용하다.

- JPA를 사용하면 엔티티 객체를 중심으로 개발
- 문제는 검색 쿼리 -> 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능 
- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요 -> SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공

```
// 검색
String jpql = "select m From Member m where m.name like '%hello%'";

List<Member> result = em.createQuery(jpql, Member,class)
    .getResultList();
```

## 문제는 동적 쿼리이다!!

### Criteria

- 문자가 아닌 자바 코드로 JPQL을 작성 할 수 있음
- JPQL 빌더 역할
- JPA 공식 기능
- **단점 : 너무 복잡하고 실용성이 없다.**
- Criteria 대신에 QueryDSL 사용 권장
- 동적 쿼리를 해결 가능하지만 너무 복잡하다.

# QueryDSL

- 문자가 아닌 자바 코드로 JPQL을 작성 할 수 있음
- JPQL 빌더 역할
- 컴파일 시점에 문법 오류를 찾을 수 있음
- 동적쿼리 작성 편리함
- 단순하고 쉬움(설정부분만 좀 힘듬)
- 실무 사용 권장
