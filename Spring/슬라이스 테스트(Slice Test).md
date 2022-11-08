* [목차](#목차)
* [슬라이스 테스트(Slice Test)](#슬라이스-테스트slice-test)
    + [슬라이스 테스트란?](#슬라이스-테스트란)
    + [API 계층 테스트](#api-계층-테스트)
        + [MemberController 테스트](#membercontroller-테스트)
        + [핵심 포인트](#핵심-포인트)
    + [데이터 액세스 계층 테스트](#데이터-액세스-계층-테스트)
        + [데이터 액세스 계층을 테스트하기 위한 한 가지 규칙](#데이터-액세스-계층을-테스트하기-위한-한-가지-규칙)
        + [MemberRepository 테스트](#memberrepository-테스트)
        + [핵심 포인트](#ed95b5ec8bac-ed8facec9db8ed8ab8-1)


# 슬라이스 테스트(Slice Test)

## 슬라이스 테스트란?

> 슬라이스 테스트란 각 계층에 구현해 놓은 기능들이 잘 자동하는지 특정 계층만 잘라서(Slice) 테스트하는 것을 말한다.
> 
- 단위 테스트만으로는 애플리케이션의 모든 기능이 정상적으로 동작한다고 보장할 수 없다.
- 하나의 애플리케이션은 계층별로 역할이 있고, 계층별로 서로 연동되기 때문에 각각의 계층 별로 잘 작동하는지 테스트를 진행한 후에 마지막으로 통합 테스트를 통해서 계층 간의 연동에 문제가 없는지 확인해야 개발의 테스트 작업이 마무리 된다.
- 개발자가 통합 테스트까지 작성하면 좋겠지만 일정 상의 이유 등으로 통합 테스트는 QA부서에서 진행하는 기능 테스트로 대체되는 경우가 많다.

## API 계층 테스트

- API 계층의 테스트 대상은 대부분 클라이언트의 요청을 받아들이는 핸들러인 Controller이다.
- Spring에서는 Controller를 테스트하기 위한 편리한 방법들을 제공한다.

```java
package com.codestates;

import com.codestates.member.dto.MemberDto;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

// @SpringBootTest 애너테이셔은 Spring Boot 기반의 애플리케이션을 테스트하기 위한 Application Context를 생성한다.
// Application Context에는 애플리케이션에 필요한 Bean 객체들이 등록되어 있다.
@SpringBootTest
/**
 * @AutoConfigureMockMvc 애너테이션은 Controller 테스트를 위한 애플리케이션의 자동 구성 작업을 해준다.
 * @AutoConfigureMockMvc 애너테이션을 추가함으로써 테스트에 필요한 애플리케이션의 구성이 자동으로 진행된다.
 * MockMvc 같은 기능을 사용하기 위해서는 @AutoConfigureMockMvc 애너테이션을 반드시 추가해야 주어야 한다.
 */
@AutoConfigureMockMvc
public class ControllerTestDefaultStructure {

    /**
     * DI로 주입 받은 MockMvc는 Tomcat 같은 서버를 실행하지 않고 Spring 기반
     * 애플리케이션의 Controller를 테스트 할 수 있는 완벽한 환경을 지원해주는 일종의 Spring MVC 테스트 프레임 워크이다.
     * MockMvc 객체를 통해 우리가 작성한 Controller를 호출해서 손쉽게 Controller에 대한 테스트를 진행할 수 있다.
      */
    @Autowired
    private MockMvc mockMvc;

    @Test
    public void postMemberTest() throws Exception {
        // given : 테스트용 request body 생성

        // when : MockMvc 객체로 테스트 대상 Controller 호출

        // Then : Controller 핸들러 메서드에서 응답으로 수신한 HTTP Status 및 response body 검증
    }
}
```

### MemberController 테스트

```java
package com.codestates.slice.controller.member;

import com.codestates.member.dto.MemberDto;
import com.google.gson.Gson;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.ResultActions;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@AutoConfigureMockMvc
public class MemberControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private Gson gson;

    @Test
    void postMemberTest() throws Exception {
        // given : request body에 포함시키는 요청 데이터와 동일한 역할을 한다.
        /**
         * Gson이라는 JSON 변환 라이브러리를 이용해서 생성한 MemberDto.Post 객체를 JSON 포맷으로 변환해준다.
         */
        MemberDto.Post post = new MemberDto.Post("gksmfcksqls@gmail.com", "김찬빈", "010-2222-2222");

        String content = gson.toJson(post);

        // when : Controller의 핸들러 메서드에 요청을 전송하기 위해서 perform() 메서드를 호출해야하고 perform() 메서드 내부에 Controller 호출을 위한 세부적인 정보들이 포함된다.
        ResultActions actions =
                mockMvc.perform(
                        /**
                         * HTTP request에 대한 정보이며, MockMvcRequestBuilders 클래스를 이용해서 빌더 패턴을 통해 request 정보를 채워 넣을 수 있다.
                         */
                        // post() 메서드를 통해 HTTP POST 메서드와 request URL을 설정한다.
                        post("/v11/members")
                                // 클라이언트 쪽에서 리턴 받을 응답 데이터 타입으로 JSON 타입을 설저한다.
                                .accept(MediaType.APPLICATION_JSON)
                                // 서버 쪽에서 처리 가능한 ContentType으로 JSON 타입을 설정한다.
                                .contentType(MediaType.APPLICATION_JSON)
                                // request body 데이터를 설정한다.
                                .content(content)
                );

        // then
        /**
         * MockMvc의 perform() 메서드는 ResultActions 타입의 객체를 리턴하는데,
         * 이 ResultActions 객체를 이용해서 우리가 전송한 request에 대한 검증을 수행할 수 있다.
         */
        MvcResult result = actions
                // andExpect() 메서드를 통해 파라미터로 입력한 매처(Matcher)로 예상되는 기대 결과를 검증할 수 있다.
                // status().isCreated()를 통해 response status가 201(Created)이 맞는지 검증하고 있다.
                .andExpect(status().isCreated())
                // JsonPath()를 사용하면 JSON 형식의 개별 프로퍼티에 손쉽게 접근할 수 있다.
                // JsonPath() 메서드를 통해 request body로 전송한 내용이 일치하는지 검증한다.
                .andExpect(jsonPath("$.data.email").value(post.getEmail()))
                .andExpect(jsonPath("$.data.name").value(post.getName()))
                .andExpect(jsonPath("$.data.phone").value(post.getPhone()))
                // andReturn()을 통해서 response 데이터를 확인할 수 있다.
                // 디버깅 용도로 response로 전달되는 응답 데이터를 출력할 때 사용할 수 있다.
                .andReturn();

        System.out.println(result.getResponse().getContentAsString());
    }
}
```

### 핵심 포인트

- 개발자가 각 계층에 구현해 놓은 기능들을 잘 동작하는지 특정 계층만 잘라서(Slice) 테스트하는 것을 슬라이스 테스트(Slice Test)라고 한다.
- `@SpringBootTest` 애너테이션은 Spring Boot 기반의 애플리케이션을 테스트하기 위한 Application Context를 생성한다.
- `@AutoConfigureMockMvc` 애너테이션은 Controller 테스트를 위한 애플리케이션의 자동 구성 작업을 해준다.
- `@MockMvc` 는 Tomcat 같은 서버를 실행하지 않고 Spring 기반 애플리케이션의 Controller를 테스트할 수 있는 완벽한 환경을 지원해주는 일종의 Spring MVC 테스트 프레임워크이다.
- MockMvc로 테스트 대상 Controller의 핸들러 메서드에 요청을 전송하기 위해서는 `perform()` 메서드를 먼저 호출해야 한다.
- MockMvcRequestBuilders 클래스를 이용해서 빌더 패턴을 통해 request 정보를 채워 넣을 수 있다.
- MockMvc의 `perform()` 메서드가 리턴하는 `ResultActions` 타입의 객체를 이용해서 request에 대한 검증을 수행할 수 있다.

## 데이터 액세스 계층 테스트

### 데이터 액세스 계층을 테스트하기 위한 한 가지 규칙

> DB의 상태를 테스트 케이스 실행 이전으로 되돌려서 깨끗하게 만든다.
> 

```java
package com.codestates;

import org.junit.jupiter.api.Test;

public class DataAccessLayerTest {
    @Test
    public void testA() {
        // 데이터가 DB에 잘 저장되는지를 테스트하기 위해 한 건의 데이터를 DB에 저장
        // DB에 잘 저장되었는지 DB에서 조회해서 결과를 확인
    }

    @Test
    public void testB() {
        // 데이터가 DB에서 잘 조회되는지를 테스트하기 위해 DB에서 조회
    }
}
```

- JUnit으로 데이터 액세스 계층을 테스트하는 이론적인 예이다.
- 일반적으로 데이터액세스 계층을 테스트하기 위해 데이터베이스에 저장하는 테스트 데이터는 테스트 케이스를 실행할 때 대부분 같은 데이터로 테스트를 진행한다.

### MemberRepository 테스트

```java
package com.codestates.slice.repository.member;

import com.codestates.member.entity.Member;
import com.codestates.member.repository.MemberRepository;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;

import java.util.Optional;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

/**
 * Spring에서 데이터 액세스 계층을 테스트하기 위한 방법은 @DataJpaTest 애너테이션을 사용하는 것이다.
 * @DataJpaTest 애너테이션을 테스트 클래스에 추가하면 MemberRepository의 기능을 정상적으로 사용하기 위한 Configuration을 Spring이 자동으로 해주게 된다.
 * @DataJpaTest 애너테이션은 @Transactional 애너테이션을 포함하고 있기 때문에
 * 하나의 테스트 케이스 실행이 종료되는 시점에 데이터베이스에 저장된 데이터를 rollback처리한다.
 * 즉, 여러 개의 테스트 케이스를 한꺼번에 실행시켜도 하나의 테스트 케이스가 종료될 때마다 데이터베이스의 상태가 초기상태를 유지한다는 것이다.
 * @DataJpaTest는 Spring Boot의 모든 자동 구성을 활성화하는 것이 아니라 데이터 액세스 계층에 필요한 자동 구성을 활성화한다.
 */
@DataJpaTest
public class MemberRepositoryTest {

    // 테스트 대상 클래스인 MemberRepository를 DI 받는다.
    @Autowired
    private MemberRepository memberRepository;

    @Test
    public void saveMemberTest() {
        // given : 테스트 할 회원 정보 데이터(member)를 준비한다.
        Member member = new Member();
        member.setEmail("gksmfcksqls@gmail.com");
        member.setName("김찬빈");
        member.setPhone("010-2222-2222");

        // when : 회원 정보를 저장한다.
        Member saveMember = memberRepository.save(member);

        // then : 회원 정보가 잘 저장되었는지 검증(Assertion)한다.
        // 반환된 Member 객체가 null이 아닌지 검증한다.
        assertNotNull(saveMember);
        // 반환된 Member 객체의 필드들이 테스트 데이터와 일치하는지 검증한다.
        assertTrue(member.getEmail().equals(saveMember.getEmail()));
        assertTrue(member.getName().equals(saveMember.getName()));
        assertTrue(member.getPhone().equals(saveMember.getPhone()));
    }

    @Test
    public void findByMemberTest() {
        // given : 테스트 할 회원 정보 데이터(member)를 준비한다.
        Member member = new Member();
        member.setEmail("gksmfcksqls@gmail.com");
        member.setName("김찬빈");
        member.setPhone("010-2222-2222");

        // when : 회원 정보를 저장한다.
        memberRepository.save(member);
        Optional<Member> findMember = memberRepository.findByEmail(member.getEmail());

        // then : 회원 정보의 조회가 정상적으로 이루어지는지 검증(Assertion)한다.
        // 같이 조회된 회원 정보가 null이 아닌지를 검증
        assertTrue(findMember.isPresent());
        // 조회된 회원의 이메일 주소와 테스트 데이터의 이메일가 일치하는지 검증한다.
        assertTrue(findMember.get().getEmail().equals(member.getEmail()));
    }
}
```

### 핵심 포인트

- 데이터 액세스 계층 테스트 시에는 테스트 종료 직후, DB의 상태를 테스트 케이스 실행 이전으로 되돌려서 깨끗하게 만든다.
- `@DataJpaTest` 애너테이션을 사용하면 Spring Data JPA 환경에서 데이터 액세스 계층의 테스트를 손쉽게 진행할 수 있다.
- `@DataJpaTest` 애너테이션은 `@Transactional` 애너테이션을 포함하고 있기 때문에 하나의 테스트 케이스 실행이 종료되는 시점에 데이터베이스에 저장된 데이터는 rollback 처리된다.