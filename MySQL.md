# DataBase - MySQL

**목차**   
[MySQL 쿼리](#mysql-쿼리)  
[MySQL 접속](#mysql-접속)  
[DataBase 생성](#database-생성)  
[사용자 생성과 권한 주기](#사용자-생성과-권한-주기)
[생성한 DB에 접속하기](#생성한-db에-접속하기)
[MySQL 연결 끊기](#mysql-연결-끊기)
[DBMS에 존재하는 데이터베이스 확인하기](#dbms에-존재하는-데이터베이스-확인하기)
[데이터베이스에 존재하는 테이블 확인하기](#데이터베이스에-존재하는-테이블-확인하기)

## **MySQL 쿼리**

- MySQL의 키워드는 대소문자를 구별하지 않는다.
- 쿼리를 이용해서 계산식의 결과도 구할 수 있다.
- 여러 문장을 한 줄에 연속으로 붙여서 실행가능하다. (각 문장에 ';'만 붙혀 주면 된다.)
- 하나의 SQL은 여러 줄로 입력가능하다.(';'콜론으로 문장 구분하기 때문에)
- SQL을 입력하는 도중에 취소할 수 있다.('\c'를 사용하면 쿼리를 즉시 취소 가능하다.)

## **MySQL 접속**

```
mysql -uroot -p
```
후에 비밀번호 입력하면 MySQL에 접속 할 수 있다.

## **DataBase 생성**

관리자 계정으로 MySQL에 접속했다면, 데이터베이스를 생성할 수 있다.

```
mysql> create database DB이름;
```
이 명령어를 통해서 DB를 생성할 수 있다.

## **사용자 생성과 권한 주기**

DB를 생성했다면, 해당 데이터베이스를 사용하는 계정을 생성해야 한다.  
또한, 이 계정이 데이터베이스를 이용할 수 있게 권한을 줘야한다.

```
mysql> grant all privileges on db이름.* to 계정이름@'%' identified by '암호';
mysql> grant all privileges on db이름.* to 계정이름@'localhost' identified by '암호';
mysql> flush privileges;
```

*는 모든 권한을 의미한다.  
@'%'는 어떤 클라이언트에서든 접근 가능하다는 의미이고, @'localhost'는 해당 컴퓨터에서만 접근 가능하다는 의미이다.  
flush privileges는 DBMS에게 적용을 하라는 의미이다.  

간혹, 8.0 이상의 버전부터 위의 코드가 아래 사진처럼 에러가 난다.

![MySQL Grant Error](./picture/MysqlGrantError.JPG)

이런 경우 user를 생성해준 다음에 권한을 부여해주면 된다.

```
mysql> create user '계정이름'@'localhost' identified by '암호';
mysql> create user '계정이름'@'%' identified by '암호';

mysql> grant all privileges on db이름.* to '계정이름'@'%';
mysql> grant all privileges on db이름.* to '계정이름'@'localhost';
```

## **생성한 DB에 접속하기**

```
mysql -h호스트명 -udb계정명 -p 데이터베이스이름
```
을 치고 db계정의 비밀번호를 입력하면, 해당 데이터베이스에 접속할 수 있다.  

## **MySQL 연결 끊기**

```
mysql> exit
mysql> QUIT
```
를 사용하면 MySQL을 종료할 수 있다.

## **DBMS에 존재하는 데이터베이스 확인하기**

```
mysql> show databases;
```
를 사용하여 현재 어떤 데이터베이스가 존재하는지 알 수 있다.  
데이터베이스를 전환하거나 사용하려면,

```
mysql> use mydb;
```
명령어를 사용하면 된다.

## **데이터베이스에 존재하는 테이블 확인하기**

```
mysql> show tables;
```
를 사용하여 현재 데이터베이스에 어떤 table이 존재하는지 알 수 있다.  
존재하는 테이블의 테이블 구조를 확인하기 위해선

```
mysql> desc 테이블명;
```
명령어를 사용하면 된다.