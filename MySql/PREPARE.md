# PREPARE

- MySQL에서 Query를 실행하지 않고 준비 해두었다 정보가 주어지면 실행하도록 하는 법

```
-- 준비 단계 --
PREPARE 쿼리이름
    FROM '쿼리 내용'; -- 쿼리의 값 부분(Column 값)에 ?를 붙여줘야 함

-- 실행 단계 --
EXECUTE 쿼리이름 USING 사용할 값;
```

```
PREPARE testQuery 
    FROM 'SELECT NAME, AGE FROM user WHERE height > ?;

SET @testVar = 160;

EXECUTE testQuery USING @testVar;
```