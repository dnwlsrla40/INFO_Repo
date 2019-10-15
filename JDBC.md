# JDBC

**목차**   
[JDBC](#jdbc)
[JDBC 사용 방법](#jdbc-사용-방법)  
- [드라이버 로드](#드라이버-로드)  
- [Connection 객체](#connection-%ea%b0%9d%ec%b2%b4)  
- [Statement 생성 및 질의 수행](#statement-생성-및-질의-수행)
- [결과 사용 및 객체 닫기](#결과-사용-및-객체-닫기)  

[Spring에서 JDBC 사용](#spring에서-jdbc-사용)
- [Spring JDBC 패키지](#spring-jdbc-패키지)
- [JDBC Template](#jdbc-template)

## **JDBC**

JDBC는 JAVA에서 데이터베이스에 접속할 수 있도록하는 자바 API이다.

JDBC는 모든 자바의 데이터 액세스 기술의 근간이 되고, 데이터베이스에서 자료를 쿼리하거나 업데이트하는 방법을 제공한다.

JDBC 클래스는 자바 패키지 java.sql과 javax.sql에 포함되어있다.

안정적이고 유연한 기술이지만, 로우레벨(반복적인 중복 코드의 사용, DB에 따라 일관성없는 정보를 가진채로 Checked Exception방법으로 처리)의 기술이다.

## **JDBC 사용 방법**

1. import java.sql.*;로 import 
2. 드라이버 로드
3. Connection객체 생성
4. Statement 객체 생성 및 질의 수행
5. SQL문에 결과물이 있다면 ResultSet 객체 생성 (주로 Select문)
6. 모든 객체 닫음(열었던 반대 순서대로)

![JDBC 클래스 생성 관계](./picture/JDBC관계.JPG)

자바 어플리케이션이 데이터베이스 연결이 필요할 때 DriverManager.getConnection() 메소드를 이용하여 JDBC연결을 할 수 있다.

### **드라이버 로드**

JDBC 드라이버는 자바 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해주는 클라이언트 사이드 어댑터이다.

따라서, 각각의 DB에 맞게 접속할 수 있는 드라이버 객체 생성한다.

```
mySQL의 경우

Class.forName("com.mysql.jdbc.Driver");

Oracle의 경우

Class.forName("oracle.jdbc.driver.OracleDriver");
```

위의 코드를 하면 Driver 객체가 메모리에 올라감

### **Connection 객체**

DB가 접속됐을 때 얻어지는 객체
DriverManager를 통해 Connection객체를 얻을 수 있다.

Connection은 interface고 각각의 DB에 따라 벤더가 구현하고 있는 객체여야 한다.  

이 벤더를 통해 라이브러리를 사용할 수 있는데 이러한 기능을 수행하는 것이 드라이버 로드라고 한다.

```
mySQL의 경우

String db_url = "jdbc:mysql://localhost/dbName";
Connection con = DriverManager.getConnection(db_url,ID,PW);

Oracle의 경우
String db_url = "jdbc:oracle:thin:@117.16.46.111:1521:xe"; // ip
Connection con = DriverManager.getConnection(db_url,ID,PW);
```

### **Statement 생성 및 질의 수행**

질의 수행을 위한 질의 상태 등을 담을 Statement 객체 생성

```
Statement stmt = con.createStatement();
```

후에 이 Statement 객체에 수행할 질의 선언 및 결과가 필요하면 결과 객체 ResultSet에 저장

```
ResultSet rs = stmt.executeQuery("select no from user");

쿼리에 따른 실행 메서드

stmt.execute("query");          // any SQL
stmt.executeQuery("query");     // SELECT
stmt.executeUpdate("query");    // INSERT, UPDATE, DELETE
```

### **결과 사용 및 객체 닫기**

```
ResultSet rs = stmt.executeQuery("select no from user");

while(rs.next())    // 결과 값이 많으면 메모리 부하적인 문제로 인해 하나씩 꺼내옴
    System.out.println(rs.getInt("no"));


Close

rs.close();
stmt.close();
con.close();
```

## **Spring에서 JDBC 사용**

위쪽의 JDBC를 사용하다 보면, 반복적인 개발 요소가 있다.
이처럼 반복적인 JDBC의 저수준 세부사항을 Spring프레임워크가 처리해준다.

![Spring이 해주는 일](./picture/Spring_JDBC.JPG)

### **Spring JDBC 패키지**

- org.springframework.jdbc.core
  - JdbcTemplate 및 관련 Helper 객체 제공
- org.springframework.jdbc.datasource
  - DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜잭션매니저 및 다양한 DataSource 구현을 제공
- org.springframework.jdbc.object
  - RDBMS 조회, 갱신, 저장등을 안전하게 사용하고 재사용 가능한 객체 제공
- org.springframework.jdbc.support
  - jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크 지원

### JDBC Template

- jdbc.core에서 가장 중요한 클래스
- 리소스 생성, 해지 처리
- 스테이먼트 생성과 실행
- SQL 조회, 업테이트, 저장 프로시저 호출, ResultSet 반복 호출 실행
- JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환

### Connection Pool

데이터를 위해 DataBase에 접근, 즉 DataBase에 연결하려 하는 것은 비용이 많이 든다.

따라서 커넥션 풀은 미리 여러개의 Connection을 만들어 맺어두었다가 필요하면 빌려주고 끝나면 반납한다.

빠르게 반납하는 것이 중요하다.

![Connection Pool](./picture/ConnectionPool.JPG)

### DataSource

DataSource는 Connection Pool을 관리하는 목적으로 사용되는 객체이다.

DataSource를 이용해 커넨션을 얻어오고 반납하는 등의 작업을 수행할 수 있다.