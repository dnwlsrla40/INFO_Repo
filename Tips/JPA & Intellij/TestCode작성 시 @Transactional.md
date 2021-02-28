# @Transactional Rollback

JPA는 하나의 로직이 하나의 Transaction 안에서 동작하므로 Test 코드에서 @Transactional을 사용해야 한다.

```
@Rnwith(SpringRunner.class)
@SpringBootTest
@Transactional
public class MemberServiceTest {

    @Autowired MemberService memberService;

    @Autowired MemberRepository memberRepository;

    @Test
    @Rollback(value = false)
    public void 회원가입() throws Exception {
        // given
        Member member = new Member();
        member.setName("Kim");

        // when
        Long saveId = memberService.join(member);

        // then
        assertEquals(member, memberRepository.findOne(saveId));
    }
}
```

이때 Test 코드의 `@Transactional` 어노테이션은 기본적으로 Test가 끝나면 Rollback을 한다. 이는 반복적인 Test를 위해서 인대 이로 인해 위의 `memberService.join(member)`를 실행해도 Insert Qeury가 DB로 날라가지 않는다.

그렇기에 실제 데이터가 재대로 들어가는 지 확인하고 싶다면 위처럼 `@Rollback(value = false)`를 적어주던가 `@Autowired EntityManager em;`으로 EntityManager를 주입받아 `em.flush();`를 통해 영속성 컨텍스트의 member 정보를 DB와 동기화 시켜 Insert Qeury가 나가게 한다.