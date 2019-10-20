# Spring JDBC

**목차**  
[Spring JDBC](#spring-jdbc)
- [Spring JDBC](#spring-jdbc)
  - [**Spring JDBC**](#spring-jdbc)
    - [**JDBC 란?**](#jdbc-%eb%9e%80)
    - [**DAO 패턴**](#dao-%ed%8c%a8%ed%84%b4)
    - [**Spring JDBC 패키지**](#spring-jdbc-%ed%8c%a8%ed%82%a4%ec%a7%80)
    - [**JDBC Template**](#jdbc-template)
    - [**Connection Pool**](#connection-pool)
    - [**DataSource**](#datasource)
    - [**사용되는 쿼리관련 메서드**](#%ec%82%ac%ec%9a%a9%eb%90%98%eb%8a%94-%ec%bf%bc%eb%a6%ac%ea%b4%80%eb%a0%a8-%eb%a9%94%ec%84%9c%eb%93%9c)


## **Spring JDBC**

스프링은 데이터베이스를 다루기위한 JDBC에 쉽게 접근하기 위해서 Spring JDBC를 제공한다.

### **JDBC 란?**

[JDBC](https://github.com/dnwlsrla40/INFO_Repo/blob/master/JDBC.md)는 JAVA에서 데이터베이스에 접속할 수 있도록하는 자바 API이다.

사용법 등을 알아보고 싶다면 위의 링크를 들어가면 된다.

위쪽의 JDBC를 사용하다 보면, 반복적인 개발 요소가 있다.
이처럼 반복적인 JDBC의 저수준 세부사항을 Spring프레임워크가 처리해준다.

![Spring이 해주는 일](./picture/Spring_JDBC.JPG)

따라서 우리는

1. 실행할 SQL 정의 (String sql = "..." 방식 or 객체 생성 후 적어주는 방식)

```
[RoleDaoSqls.java]
public class RoleDaoSqls {
	public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";
	public static final String UPDATE = "UPDATE role SET description = :description WHERE ROLE_ID = :roleId";
	public static final String SELECT_BY_ROLE_ID = "SELECT role_id, description FROM role WHERE role_id = :roleId";
	public static final String DELETE_BY_ROLE_ID = "DELETE FROM role WHERE role_id = :roleId";
} 

후에 사용하는 곳에서 import static RoleDaoSqls.*;를 해주어야 한다.
```

2. 바인딩될 파라미터 설정

```
SqlParameterSource params = new BeanPropertySqlParameterSource(role);

or

Map<String, ?> params = Collections.singletonMap("roleId", id);
```

3. 넘겨받을 객체 설정

만 해주면 데이터를 사용할 수 있다.

### **DAO 패턴**

Spring은 비지니스 로직과 데이터 액세스 로직을 분리하기 위하여 [DAO](https://github.com/dnwlsrla40/INFO_Repo/blob/master/DAO.md) 패턴을 사용한다.

이를 통해 서비스 계층에 영향을 주지 않고 데이터 액세스가 가능하다.

### **Spring JDBC 패키지**

- org.springframework.jdbc.core
  - JdbcTemplate 및 관련 Helper 객체 제공
- org.springframework.jdbc.datasource
  - DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜잭션매니저 및 다양한 DataSource 구현을 제공
- org.springframework.jdbc.object
  - RDBMS 조회, 갱신, 저장등을 안전하게 사용하고 재사용 가능한 객체 제공
- org.springframework.jdbc.support
  - jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크 지원

### **JDBC Template**

- jdbc.core에서 가장 중요한 클래스
- 리소스 생성, 해지 처리
- 스테이먼트 생성과 실행
- SQL 조회, 업테이트, 저장 프로시저 호출, ResultSet 반복 호출 실행
- JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환

Spring JDBC의 모든 기능을 최대한 활용할 수 있는 유연성을 제공한다.  
실행,조회,배치 세가지 작업이 있다.

### **Connection Pool**

데이터를 위해 DataBase에 접근, 즉 DataBase에 연결하려 하는 것은 비용이 많이 든다.(실제 SQL실행시간보다 커넥션 객체 생성시간이 더 걸리게 된다.)

따라서 커넥션 풀은 미리 여러개의 Connection을 만들어 맺어두었다가 필요하면 빌려주고 끝나면 반납한다.

빠르게 반납하는 것이 중요하다.

다중 사용자를 갖는 엔터프라이즈시스템 즉, 웹어플리케이션에서는 반드시 DB커넥션 풀링을 지원하는 DataSource를 사용해야 한다.

![Connection Pool](./picture/ConnectionPool.JPG)

### **DataSource**

DataSource는 Connection Pool을 관리하는 목적으로 사용되는 객체이다.

DataSource를 이용해 커넨션을 얻어오고 반납하는 등의 작업을 수행할 수 있다.

스프링에서는 DataSource를 공유가능한 Spring Bean으로 등록해 사용할 수 있도록 해준다.

```
@Bean
public RoleDao(DataSource dataSource) {
		this.jdbc = new NamedParameterJdbcTemplate(dataSource); // JDBC Template 클래스 생성
		this.insertAction = new SimpleJdbcInsert(dataSource)
				.withTableName("role");
		
	}
```

### **사용되는 쿼리관련 메서드**

- queryForObject() 메서드 : 여러개의 칼럼, 한 개의 로우로 반환될 때 사용(id를 통한 조회)
- query() 메서드 : 여러개의 칼럼, 여러 개의 로우로 반환될 때 사용한다. (전체 조회)
- update() 메서드 : 파라미터 갯수는 가변인자(한개든 여러개든)가능하다. 결과값은 반영된 갯수이다.

```

queryForObject() 사용

public UserVO read(String id){
    String SQL = "select * from USERS where userid = ?"
    try{
        UserVO user = jdbcTemplate.queryForObject(SQL, new Objec[]{id}, new UserMapper());
        return user; 
    }catch(EmptyResultDataAccessException e){
        return null;
    }
}

query() 사용

public List<UserVO> readAll(){
    String SQL = "select * from USERS";
    List<UserVO> userList = jdbcTemplate.query(SQL, new UserMapper());
    return userList;
}

update() 사용

public void insert(UserVO user){
    String SQL = "insert into USERS (userid, name) vlaues (?,?)";
    jdbcTemplate.update(SQL,user.getUserId(),user.getName());
}
```