# @Transactional(readOnly = true)

JPA의 하나의 로직은 하나의 Transaction 안에서 실행되야 하므로 아래 코드와 같이 Service에 @Transactional을 붙여야 한다.

```
@Service
@RequiredArgsConstructor
public class MemberService {
    // 필드 Injection
    private final MemberRepository memberRepository;

    /*
     * 회원 가입
     */
    @Transactional
    public Long join{
        ...
    }

    /*
     * 회원 전체 조회
     */
    @Transactional
    public List<Member> findMembers(){
        ...
    }

    /*
     * 회원 조회
     */
    @Transactional
    public Member findOne(Long memberId){
        ...
    }
}
```

하지만, 만약 주요 로직들이 findMembers와 같이 DB 변경이 없이 조회 로직들로 되어있다면, 아래 처럼 MemberService 자체에 `@Transactional(readOnly = true)`를 통해 전체 service 로직에 Transaction을 쓰기 전용으로 설정하고 join과 같은 로직에만 따로 `@Transactional`을 선언해 주는 것이 좋다. join같은 경우 자신의 위에 달려있는 `@Transactional`이 전체 `@Transactional(readOnly = true)` 보다 우선권을 가진다.

```
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class MemberService {

    // 필드 Injection
    private final MemberRepository memberRepository;

    /*
     * 회원 가입
     */
    @Transactional
    public Long join{
        ...
    }

    /*
     * 회원 전체 조회
     */

    public List<Member> findMembers(){
        ...
    }

    /*
     * 회원 조회
     */
    public Member findOne(Long memberId){
        ...
    }
}
```