# 테스트 코드 자동완성 tdd + tab키 설정

1. ctrl + alt + s로 설정 파일을 연다.
2. 검색 창에 template을 검색하고 Live Templates를 누르고 custom에 추가한다.
3. 아래와 같이 자동 완성 코드를 등록 한다.
4. Edit variables를 눌러 default value와 Expression 규칙을 설정한다.

```
Abbreviation - 등록할 자동 완성 key
Description - 자동 완성의 설명

Template text:

@Test
public void $Name$ throws Exception {
    // given
    $END$  // 자동 완성 후 커서 위치
    // when

    // then
}
```

![Live Templates](/picture/Live_Templates.PNG)