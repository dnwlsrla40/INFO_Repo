# DELIMITER

- 구문의 끝을 설정해주는 것
- DELIMITER (설정문자)로 설정을 하면 `;`이 아닌 설정 문자로 구문 구분
- 즉, BEGIN ~ END에서 ;를 사용해도 쿼리가 끝나지 않음

## DELIMITER 사용

DELIMITER $$ or DELIMITER // or DELIMITER (아무거나) ...


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