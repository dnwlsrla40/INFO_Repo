# CHAR와 VARCHAR 차이점

## CHAR

- 고정길이 문자열 타입
- 최대 길이 255
- 타입의 크기만큼 데이터가 들어오지 않은 경우 이후 공간을 스페이스로 채움(mysql8.0(정확히 몇 버전 부터인지는 모름)에서는 select 시 후행 공백을 지우고 가져옴)
- 고정길이 문자열 타입이므로 헤더에 레코드의 길이에 대한 정보가 들어있지 않다.

## VARCHAR

- 가변길이 문자열 타입
- 최대 길이 255 (5.0.3 이후 65,535까지 가능)
- 타입의 크기만큼 데이터가 들어오지 않으면 그 크기에 맞춰 공간 할당
- 헤더에 레코드의 길이 정보가 포함(255 미만인 경우 1byte, 255 이상인 경우 2byte)

![varchar char 차이](/picture/varchar_char_차이.PNG)
![varchar char 차이2](/picture/char_varchar_document.PNG)

## 실습

![testtbl table](/picture/testtbl.PNG)
![testtbl table row](/picture/test_column.PNG)

MySQL 8.0버전에서는 CHAR 라고 하더라도 후행 공백을 지우고 데이터를 가져온다. 이를 해결하려면 sql_mode에 "PAD_CHAR_TO_FULL_LENGTH" 설정을 주면 된다.

**sql_mode 설정 전**  
![sql_mode 설정 전](/picture/sql_mode.PNG)

```
select char_length(char_column), char_length(varchar_column) from testtbl;
select length(char_column), length(varchar_column) from testtbl;
```
**sql_mode 설정 전 쿼리 결과**  
![sql_mode 설정 전 쿼리 결과](/picture/sql_mode_설정전_쿼리결과.PNG)

**sql_mode 설정**  
![sql_mode 설정](/picture/sql_mode_설정.PNG)

```
select char_length(char_column), char_length(varchar_column) from testtbl;
select length(char_column), length(varchar_column) from testtbl;
```

**sql_mode 설정 후 쿼리 결과**  
![sql_mode 설정 후 쿼리 결과](/picture/sql_mode_설정후_쿼리결과.PNG)

이와 같이 "sql_mode"를 설정해주면 CHAR Type인 경우 공백을 차지하는 것을 볼 수 있다.  
또한, 여기서 `length(varchar())`값이 4가 나오는 이유는 `length()` 함수는 String의 byte 길이만 반환 하므로 헤더의 "data_len"의 1byte는 포함하지 않기 때문이다.

**출처**

mysql document : https://dev.mysql.com/doc/refman/8.0/en/char.html
