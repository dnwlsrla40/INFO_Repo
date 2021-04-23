# LOAD_FILE로 대용량 데이터 INSERT시 NULL값 으로 들어가는 현상

위와 같은 문제가 발생하는 데에는 2가지 이유가 있을 수 있다.

1. MySQL DB 기본 대용량 업로드 제한이 4MB로 지정되어 있다.

2. 경로로 지정해준 폴더가 위험하지 않은 경로라고 알려주어야 한다.

## 1번 방법 해결

1. `show variables like 'max_allowed_packet';`를 통해서 보면 packet 허용량이 약 4MB로 제한되어 있다.

    ![packet 허용량](/picture/max_allowed_packet.PNG)

2. 이 packet 허용량을 늘려주면 된다. 이를 늘려주려면 my.ini(window 기준) 파일에서 MySQL 설정을 변경해주어야 하는데 이때 관리자 권한이 있어야만 파일을 변경할 수 있다.

3. 시작 메뉴에서 마우스 오른쪽 버튼으로 POWER CELL을 관리자 모드로 실행해준뒤 my.ini 파일이 있는 곳까지 이동한다.

    ![PowerShell 관리자](/picture/PowerShell_관리자모드.PNG)
    ![MySQL 설정파일 경로](/picture/mysql_설정파일_경로.PNG)

4. 이후 'my.ini'파일이 있는 곳에서 `NOTEPAD my.ini`로 파일을 열고 'ctrl + f' 버튼으로 `max_allowed_packed`부분을 찾아서 100으로 되어 있는 부분을 1024M으로 바꾼다.(자신이 원하는 만큼)

5. 설정을 적용하기 위해 `NET STOP MySQL(80은 버전에 따라 붙임)`과 `NET START MySQL80`으로 MySQL 재시작 해준다.

## 2번 방법 해결

1. `show variables like 'secure_file_priv';`를 통해서 보면 허용된 경로가 "MySQL Sever8.0" 경로 뿐이다.

    ![허용된 경로](/picture/secure_file_priv.PNG)

2. 1번 방법의 2,3,4번과 같은 방법으로 my.ini를 변경해준다. (`max_allowed_packed`대신 `secure_file_priv`로 찾고 경로 추가)

3. 설정을 적용하기 위해 `NET STOP MySQL(80은 버전에 따라 붙임)`과 `NET START MySQL80`으로 MySQL 재시작 해준다.


## 해결 결과

![바뀐 결과 값](/picture/설정파일_변경_후.PNG)