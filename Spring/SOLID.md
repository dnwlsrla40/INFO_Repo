# SOLID

- 클린코드로 유명한 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리

**SOLID**
- SRP(single responsibility principle) : 단일 책임 원칙
- OCP(open/closed principle) : 개방-폐쇄 원칙
- LSP(Liskov substitution principle) : 리스코프 치환 원칙
- ISP(Interface segregation principle) : 인터페이스 분리 원칙
- DIP(Dependency inversion principle) : 의존관계 역전 원칙

## SRP 단일 책임 원칙

- **한 클랙스는 하나의 책임만 가져야 한다.**
- 하나의 책임이라는 것은 모호하다.
    - 클 수도 있고, 작을 수도 있다.
    - 문맥과 상황에 따라 다르다.
- 중요한 기준은 변경!!!
    - 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
- ex) UI 변경, 객체의 생성과 사용을 분리 

## OCP 개방-폐쇄 원칙

- **소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.**
- 다형성을 활용(인터페이스를 추가로 생성해 확장하는 등...)
- 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
- 역할과 구현은 분리되어야 한다!!!

**문제점**
- MemberService 클라이언트가 구현 클래스를 직접 선택
```
MemberRepository m = new MemoryMemberRepository(); // 기존 코드
MemberRepository m = new JdbcMemberRepository(); // 변경 코드
```
- 구현 객체를 변경하려면 클라이언트 코드를 변경해야 한다.
    - 다형성을 사용했지만 OCP 원칙을 지킬 수 없다. -> 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요 -> 스프링 컨테이너의 역할(IOC, DI etc...)

## LSP 리스코프 치환 원칙

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
- 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다.
- 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체는 믿고 사용하려면, 이 원칙이 필요하다.
- **즉, 인터페이스 구현체는 인테페이스 규약을 지켜야한다!!**
    - ex) 자동차의 인터페이스에 엑셀은 앞으로 가라는 기능으로 되어있으면, 느리더라도 앞으로 가야한다는 것, 뒤로 가게 구현할 경우 LSP 위반

## ISP 인터페이스 분리 원칙

- **특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.**
    - ex) 자동차 인터페이스 -> 운전 인터페이스, 정비 인터페이스로 분리 -> 사용자 클라이언트를 운전자 클라이언트와 정비사 클라이언트로 분리 가능
- 분리하면 정비 인터페이스 자체가 변해도 운전자 클라이언트에 영향을 주지 않음
- 인터페이스가 명확해지고, 대체 가능성이 높아짐

## DIP 의존관계 역전 원칙

- 프로그래머는 "추상화에 의존해야지, 구체화에 의존하면 안된다"
    - ex) 의존성 주입(이 원칙을 따른 방법)
- **즉, 구현 클래스가 아닌 인터페이스에 의존!!!**
- 역할(Role)에 의존하게 해야 한다. 
    - 클라이언트가 인터페이스에 의존해야 유연하게 구현체를 변경할 수 있다!

**DIP 읜존관계 역전 원칙(아까의 문제점 다시 고찰)**

```
MemberRepository m = new MemoryMemberRepository(); // 기존 코드
MemberRepository m = new JdbcMemberRepository(); // 변경 코드
```

- 위의 코드는 인테페이스에 의존하지만, 구현 클래스도 동시에 의존한다.
- MemberService 클라이언트가 구현 클래스를 직접 선택
    - `MemberRepository m = new MemoryMemberRepository();`
    - 후에 서버 코드 변경 시 클라이언트 코드 역시 변경해야 함
        - ex) `MemberRepository m = new ChangeMemberRepository();`
    - DIP 위반
        - 해결하기 위해 Spring은 Spring Container를 통해 IoC, DI 등의 기능 지원  

## 정리

- 객체 지향의 핵심은 다형성
- 하지만 다형성 만으로는 쉽게 부품을 갈아 끼우듯이 개발할 수 없다. (OCP, DIP 원칙 지킬 수 없다.)
- 다형성 만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경된다.
- 즉, 무언가 더 필요하다.

## [참고 자료 및 출처]

- 김영한 님의 스프링 핵심 원리 - 기본편 강의