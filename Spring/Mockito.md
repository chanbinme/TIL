* [목차](#목차)
* [Mockito](#mockito)
    + [Mock이란?](#mock이란)
        + [테스트 세계에서의 Mock](#테스트-세계에서의-mock)
    + [테스트에서 Mock 객체를 사용하는 이유](#테스트에서-mock-객체를-사용하는-이유)
        + [Mock 객체를 사용하지 않은 슬라이스 테스트 프로세스](#mock-객체를-사용하지-않은-슬라이스-테스트-프로세스)
        + [Mock 객체를 사용한 슬라이스 테스트 프로세스](#mock-객체를-사용한-슬라이스-테스트-프로세스)
    + [Mockito란?](#mockito란)
        + [슬라이스 테스트에 Mockito 적용](#슬라이스-테스트에-mockito-적용)
    + [핵심 포인트](#핵심-포인트)

# Mockito

## Mock이란?

> 진짜 제품은 아니지만 실제 제품과 유사한 디자인을 가질 수도 있고, 실제 제품처럼 전체 기능이 동작하지는 않지만 일부 기능을 테스트해 볼 수 있는 모형 제품
> 

### 테스트 세계에서의 Mock

> 테스트 세계에서의 Mock은 가짜 객체를 의미한다.
> 
- 단위 테스트나 슬라이스 테스트 등에 Mock 객체를 사용하는 것을 Mocking이라고 한다.

## 테스트에서 Mock 객체를 사용하는 이유

### Mock 객체를 사용하지 않은 슬라이스 테스트 프로세스

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/256a5a10-65fe-4636-97aa-105d519bf38f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221111T012037Z&X-Amz-Expires=86400&X-Amz-Signature=1cdab38b9cf50b97d24fe20b0d7e9ac85a86b814851ed5ccfa192a60964a1eb7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Controller의 PostMember() 핸들러 메서드만 테스트를 해야되는데 서비스 계층을 거쳐서 데이터 액세스 계층 그리고 데이터베이스까지 동작 흐름이 이어지기 때문에 슬라이스 테스트라기 보다는 통합 테스트에 가깝다.
- 슬라이스 테스트의 목적은 해당 계층 영역에 대한 테스트에 집중하는 것이다.
- 하나의 계층만 테스트하면 되는데 불필요한 전 계층을 다 거치게되면 성능이나 테스트 관심 영역 면에서 슬라이스 테스트의 주 목적에 맞지 않는다.

### Mock 객체를 사용한 슬라이스 테스트 프로세스

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6efbc0ec-8bdb-4076-87b5-787ee7f966fc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221111T012059Z&X-Amz-Expires=86400&X-Amz-Signature=1726d0996b31504f181805ca25741fada15b2345d3beeec1060160ecffe5fe36&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Mock 객체를 사용하면 Controller에서 Service의 createMember()를 호출하지 않고 MockService 클래스에서 createMember()를 호출하고 있다.
- Mock 객체를 이용함으로써 다른 계층과 단절되어 불필요한 과정을 줄일 수 있다.

## Mockito란?

> Mockito는 Spring Framework 자체적으로 지원하고 있는 Mocking 라이브러리이다.
> 

## 슬라이스 테스트에 Mockito 적용

### MemberController의 PostMember() 테스트에 Mockito 적용

```java
package com.codestates.slice.mock;

import com.codestates.member.dto.MemberDto;
import com.codestates.member.entity.Member;
import com.codestates.member.mapper.MemberMapper;
import com.codestates.member.service.MemberService;
import com.codestates.stamp.Stamp;
import com.google.gson.Gson;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.ResultActions;

import static org.mockito.BDDMockito.given;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@AutoConfigureMockMvc
public class MemberControllerMockTest {
    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private Gson gson;

    /**
     * @MockBean 애너테이션은 Application Context에 등록되어 있는 Bean에 대한
     * Mockito Mock 객체를 생성하고 주입해주는 역할을 한다.
     * @MockBean 애너테이션을 필드에 추가하면 해당 필드의 Bean에 대한 Mock 객체를 생성한 후, 필드에 주입한다.
     * 여기서는 MemberService 빈에 대한 Mock 객체를 생성해서 memberSerivice 필드에 주입한다.
     */
    @MockBean
    private MemberService memberService;

    // MemberMapper를 DI 받는 이유는 MockMemberService의 createMember()에서 리턴하는 Member 객체를 생성하기 위함이다.
    @Autowired
    private MemberMapper mapper;

    @Test
    void postMemberTest() throws Exception {
        // given
        MemberDto.Post post = new MemberDto.Post("gksmfcksqls@gmail.com",
                "김찬빈",
                "010-2222-2222");

        Member member = mapper.memberPostToMember(post);    // mapper를 이용해 post 변수를 Member 객체로 변환한다.
        member.setStamp(new Stamp());   // stamp를 추가해주지 않으면 MemberResponseDto 클래스 객체가 JSON으로 변환되는 과정 중에 Stamp에 대한 정보가 없다는 예외가 발생한다.

        /**
         * given()은 Mock 객체가 특정 값을 리턴하는 동작을 지정하는데 사용하며, Mockito에서 지원하는 when()과 동일한 기능을 한다.
         * 여기서는 MockMemberService 객체로 createMember() 메서드를 호출하도록 정의하고 있다.
         *
         * Mockito.any(Member.class)는 Mockito에서 지원하는 변수 타입 중 하나이다.
         * 실제 MemberService 클래스에서 createMember() 파라미터는 Member타입이기 때문에 Mockito.any()에 Member.class 타입을 지정해주었다.
         *
         * .willReturn(member)는 MockMemberService의 createMember() 메서드가 리턴할 Stub 데이터이다.
         */
        given(memberService.createMember(Mockito.any(Member.class)))
                .willReturn(member);

        String content = gson.toJson(post);

        // when
        ResultActions actions =
                mockMvc.perform(
                        post("/v11/members")
                                .accept(MediaType.APPLICATION_JSON)
                                .contentType(MediaType.APPLICATION_JSON)
                                .content(content)
                );

        // then
        MvcResult result = actions
                .andExpect(status().isCreated())
                .andExpect(jsonPath("$.data.email").value(post.getEmail()))
                .andExpect(jsonPath("$.data.name").value(post.getName()))
                .andExpect(jsonPath("$.data.phone").value(post.getPhone()))
                .andReturn();
    }

}
```

- Stubbling이란 테스트를 위해서 Mock 객체가 항상 일정한 동작을 하도록 지정하는 것을 의미한다.

```java
2022-11-10 11:30:40.036  INFO 81768 --- [ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
2022-11-10 11:30:40.038  INFO 81768 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
2022-11-10 11:30:40.044  INFO 81768 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
BUILD SUCCESSFUL in 13s
```

- 실제로 MemberService의 createMember()가 호출되면 Member, Stamp 등록 등의 Insert 쿼리문이 출력되지만 MockMemberService의 createMember()가 호출되었기 때문에 별도 등록에 대한 쿼리가 출력되지 않는다.
- **즉, MockMemberService 클래스는 우리가 테스트하고자 하는 Controller의 테스트에 집중할 수 있도록 다른 계층과의 연동을 끊어주는 역할을 한다.**

### ****MemberService의 createMember() 테스트에 Mockito 적용****

- 일반적으로 비즈니스 로직은 데이터 액세스 계층과는 무관하게 서비스 계층에 구현된 비즈니스 로직 자체를 Spring Framework의 도움을 받지 않고도 빠르게 테스트를 진행할 수 있어야 한다.

```java
@Transactional
@Service
public class MemberService {
    private final MemberRepository memberRepository;
    private final ApplicationEventPublisher publisher;

    public MemberService(MemberRepository memberRepository,
                         ApplicationEventPublisher publisher) {
        this.memberRepository = memberRepository;
        this.publisher = publisher;

    }

    public Member createMember(Member member) {
        verifyExistsEmail(member.getEmail());     // (1)
        Member savedMember = memberRepository.save(member);

        publisher.publishEvent(new MemberRegistrationApplicationEvent(this, savedMember));
        return savedMember;
    }

    ...
		...

    private void verifyExistsEmail(String email) {
        Optional<Member> member = memberRepository.findByEmail(email);  // (2)

        // (3)
        if (member.isPresent())
            throw new BusinessLogicException(ExceptionCode.MEMBER_EXISTS);
    }
}
```

- DB에 존재하는 이메일인지 여부를 검증하는 `verifyExistsEmail()` 메서드가 정상적인 동작을 수행하는지를 테스트할 것이다.
- `verfiExistsEmail()` 은 `memberRepository.findByEmail(email)` 을 통해 DB에서 회원을 조회하고 있다.
- 하지만 우리는 DB에서 Member 객체를 잘 조회하는지 여부를 테스트하려는게 아니라, 어디서 조회해 왔든 상관없이 조회된 Member 객체가 null이면 `BusinessLogicException` 을 잘 던지는지 여부를 테스트하면 된다.

```java
package com.codestates.slice.mock;

import com.codestates.exception.BusinessLogicException;
import com.codestates.member.entity.Member;
import com.codestates.member.repository.MemberRepository;
import com.codestates.member.service.MemberService;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import java.util.Optional;

import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.mockito.BDDMockito.given;

// Spring을 사용하지 않고, JUnit에서 Mockito의 기능을 사용하기 위해서 아래 애너테이션을 추가해야 한다.
@ExtendWith(MockitoExtension.class)
public class MemberServiceMockTest {

    // @Mock 애너테이션을 추가하면 해당 필드의 객체를 Mock 객체로 생성한다.
    @Mock
    private MemberRepository memberRepository;

    // @InjectMocks 애너테이션을 추가한 필드에 Mock 객체를 주입해준다.
    // 여기서는 MemberService 객체는 주입 받은 MemberRepository Mock 객체를 포함하고 있다.
    @InjectMocks
    private MemberService memberService;

    @Test
    public void createMemberTest() {
        // given
        Member member = new Member("gksmfcksqls@gmail.com", "김찬빈", "010-2222-2222");

        // memberRepository Mock 객체로 Stubbling을 하고 있다.
        // Optional.of(member)의 member 객체가 포함된 이메일 주소가 파라미터로 전달한 member 객체에 포함된 이메일 주소가 동일하기 때문에 passed이다.
        given(memberRepository.findByEmail(member.getEmail()))
                .willReturn(Optional.of(member));

        // when / then
        assertThrows(BusinessLogicException.class, () -> memberService.createMember(member));
    }
}
```

## 핵심 포인트

- 테스트 세계에서의 Mock은 가짜 객체를 의미한다.
- Mockito는 Mock 객체를 생성하고, 해당 Mock 객체가 진짜처럼 동작하게 하는 기능을 하는 Mocking Framework이다.
- `@MockBean` 애너테이션은 Application Context에 등록되어 있는 Bean에 대한 Mockito Mock 객체를 생성하고 주입해주는 역할을 한다.
- JUnit에서 Spring을 사용하지 않고 순수하게 Mockito의 기능만을 사용하기 위해서는 `@ExtendWith(MockitoExtentsion.class)` 를 클래스 레벨에 추가해야 한다.
- `@Mock` 애너테이션을 추가하면 해당 필드의 객체를 Mock 객체로 생성한다.
- `@Mock` 애너테이션을 통해 생성된 Mock 객체는 `@InjectMocks` 애너테이션을 추가한 필드에 주입된다.