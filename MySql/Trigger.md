# 트리거(Trigger)

- 테이블에 대한 이벤트(DML 실행, DDL 실행, 데이터베이스 동작 실행(log on, log off, start up, shutdown, server error))에 반응해 자동으로 실행되는 작업
- DML(데이터 조작 언어)의 데이터 상태 관리를 자동화하는 데 사용

**속성**

- BEFORE or AFTER : 트리거가 실행되는 시기 지정
- INSTEAD OF : 트리거를 원래 문장 대신 실행
- WHEN : 트리거를 시작하는 조건식 지정

## 문장 트리거

- 각 행에 대해서 트리거가 발생

## 행 트리거

- 전체 트랜젝션 작업에 대해 1번 발생

## 트리거 생성

```
DELIMITER //
CREATE TRIGGER 트리거 이름
    AFTER(BEFORE) INSERT(DELECT, ALTER ...)
    ON 테이블 이름
    FOR EACH ROW(행 트리거 일 경우)
BEGIN -- 트리거 시작
    트리거 내용
END -- 트리거 끝
DELIMITER ;
```


```
DELIMITER //
CREATE TRIGGER trg_deletedMemberTBL -- 트리거 이름
	ALTER DELETE -- 삭제 후에 작동하게 지정
    ON memberTBL -- 트리거를 부착할 테이블
    FOR EACH ROW -- 각 행마다 적용
BEGIN
	-- OLD 테이블의 내용을 백업 테이블에 삽입
    INSERT INTO deletedMemberTBL
		VALUES (OLD.memberID, OLD.memberName, OLD.memberAddress, CURDATE() );
END //
DELIMITER ;
```

## 트리거 확인

```
SHOW TRIGGERS;
```

## 트리거 삭제

```
DROP TRIGGERS 트리거 이름;
```