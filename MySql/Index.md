# 인덱스(Index)

- 테이블에서 원하는 데이터를 쉽고 빠르게 찾기 위해 사용
- 자주 사용되는 필드 값으로 만들어진 원본 테이블의 사본(가상의 테이블을 생성한다 생각)
- 사용자가 직접 접근할 수 없으며, 검색과 질의에 대한 처리에서만 사용

>
> **중요**  
> 인덱스가 설정된 필드 값을 포함한 데이터의 삽입, 삭제, 수정 작업이 원본 테이블에서 이루어질 경우, 인덱스도 함께 수정되어야 한다. 따라서 수정보다는 검색이 자주 사용되는 테이블에서 사용하는 것이 좋다.

## UNIQUE INDEX

- Index에서 필드 값은 중복 될 수 없다.
- NULL 값은 가질 수 있다.

## FULLTEXT INDEX

- 일반적인 인덱스와 다르게 매우 빠르게 테이블의 모든 텍스트 필드를 검색
- 검색 엔진과 유사한 방법으로 자연어를 이용하여 데이터를 검색할 수 있도록 모든 데이터의 문자열 단어를 저장

## Index 생성

```
CREATE INDEX 인덱스 이름
ON 테이블 이름 (필드 이름1, 필드 이름2, ... + DESC or ASC로 정렬 가능)


CREATE INDEX NameDescIdx
On Reservation (Name DESC);
```

## Index 정보 보기

위의 코드로 생성한 인덱스는 아래와 같은 문법을 통해 확인 가능

```
SHOW INDEX
FROM 테이블 이름
```
|속성|설명|
|:---:|:---|
|Table|테이블의 이름|
|Non_unique|인덱스가 중복된 값을 저장할 수 있으면 1, 없으면 0|
|Key_name|인덱스의 이름, 인덱스가 해당 테이블의 기본 키면 PRIMARY로 표시|
|Seq_in_index|인덱스에서의 해당 필드의 순서를 표시|
|Column_name|해당 필드의 이름|
|Collation|인덱스에서 해당 필드가 정렬되는 방법|
|Cardinality|인덱스에 저장된 유일한 값들의 수|
|Sub_part|인덱스 접두어|
|Packed|키가 압축(packed)되는 방법|
|NULL|해당 필드가 NULL을 저장할 수 있으면 YES, 없으면 ""|
|Index_type|인덱스에 사용되는 메소드|
|Comment|해당 필드를 설명하는 것이 아닌 인덱스에 관한 기타 정보|
|Index_comment|인덱스에 관한 모든 기타 정보|

## Index 추가

**추가할 수 있는 인덱스 Type**

1. 기본 인덱스
2. UNIQUE INDEX
3. FULLTEXT INDEX

### 기본 인덱스 추가

```
ALTER TABLE 테이블 이름
ADD INDEX 인덱스 이름(필드 이름)
```

### UNIQUE INDEX 추가

```
ALTER TABLE 테이블 이름
ADD UNIQUE 인덱스 이름(필드 이름)
```

### FULLTEXT INDEX 추가

```
ALTER TABLE 테이블 이름
ADD FULLTEXT INDEX이름 (필드 이름)
```