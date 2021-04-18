# Select 시 조건

## Where 절

### BETWEEN ... AND

- A에서 B사이 **연속된** 값을 조회할 경우 사용

```
SELECT * FROM user WHERE age BETWEEN 20 AND 25;
```

### IN()

- 주로 연속된 값이 아닌 여러 개 값 중 속한 경우 조회할 때 사용 

```
SELECT * FROM user WHERE area IN ('서울', '경남', '경기');
```

### LIKE

- 문자열 내용을 검색할 때 사용
- % : 무엇이든 허용
- _ : 한 글자 아무거나

```
SELECT * FROM user WHERE name LIKE '홍%';

SELECT * FROM user WHERE name LIKE '_길동';
```

### ANY

- 서브 쿼리의 조건으로 사용
- 서브(하위) 쿼리가 둘 이상의 값을 반환하는 경우 ANY를 써서 나온 값중 하나라도 만족하는 경우를 다 조회한다.(or 과 동일)
- `=ANY(서브쿼리)`는 `IN(서브쿼리)`와 동일한 의미를 가진다.
- `SOME`은 `ANY`와 비슷한 역할을 한다.

```
SELECT * FROM user WHERE age > ANY (SELECT age FROM user WHERE phone = "011"); // 번호가 011인 사람의 age 보다 큰 경우 전부 조회
```

### ALL

- 서브 쿼리의 조건으로 사용
- 서브(하위) 쿼리가 둘 이상의 값을 반환하는 경우 ALL를 써서 나온 값 모두 만족하는 경우를 조회한다.(and 과 동일)

```
SELECT * FROM user WHERE age > ANY (SELECT age FROM user WHERE phone = "011"); // 번호가 011인 사람 전부의 age보다 큰 경우 조회
```

## 정렬

### ORDER BY

- 원하는 순서대로 정렬
- MySQL 성능을 상당히 떨어뜨릴 소지가 있어 꼭 필요한 거 아닌 경우 되도록 사용하지 않는 것이 좋음
- DESC : 내림차순
- ASC : 오름차순(기본 값)

```
SELECT * FROM user ORDER BY age DESC, height ASC; // 나이 순 내림차순 정렬 후 같은 나이인 경우 키 순으로 정렬
```

## 중복 제거

### DISTINCT

- 결과가 중복된 경우 하나만 출력

```
SELECT DISTINCT age FROM user;
```

## 출력 갯수 제한

### LIMIT

- `LIMIT N` 구문으로 조회한 결과 값 중 상위 N개만 출력할 수 있다.
- `LIMIT N, M;` 구문으로 N부터 시작하여 M개까지 출력한다.

## GROUP BY 절

## HAVING 절

- WHERE 절과 비슷한 개념으로 조건을 제한
- 집계 함수에 대한 조건을 제한
- **GROUP BY 다음에 나와야 한다.**