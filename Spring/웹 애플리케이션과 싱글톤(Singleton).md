* [목차](#목차)
* [웹 애플리케이션과 싱글톤](#웹-애플리케이션과-싱글톤)
    + [웹 애플리케이션과 싱글톤](#웹-애플리케이션과-싱글톤)
        + [스프링 없는 순수한 DI 컨테이너 테스트](#스프링-없는-순수한-di-컨테이너-테스트)
    + [싱글톤 패턴](#싱글톤-패턴)
        + [싱글톤을 사용한 테스트](#싱글톤을-사용한-테스트)
    + [싱글톤 문제점](#싱글톤-문제점)
    + [싱글톤 컨테이너](#싱글톤-컨테이너)
* [@Configuration과 싱글톤](#configuration과-싱글톤)
    + [@Configuration과 바이트코드 조작](#configuration과-바이트코드-조작)
    
# 웹 애플리케이션과 싱글톤

## 웹 애플리케이션과 싱글톤

- 스프링은 기업용 온라인 서비스를 위해 탄생했다.
- 대부분의 스프링 애플리케이션은 웹 애플리케이션이다.
- 웹 애플리케이션은 일반적으로 여러 고객이 동시에 요청을 한다.

### 스프링 없는 순수한 DI 컨테이너 테스트

```java
public class SingletonTest {

    @Test
    @DisplayName("스프링 없는 순수한 DI 컨테이너")
    void pureContainer() {
        AppConfig appConfig = new AppConfig();

        // 1. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService1 = appConfig.memberService();

        // 2. 조회: 호출할 때 마다 객체를 생성
        MemberService memberService2 = appConfig.memberService();

        // 참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        // memeberService != memberService2
        Assertions.assertThat(memberService1).isNotSameAs(memberService2);
    }
}

// 결과
memberService1 = hello.core.member.MemberServiceImpl@22fcf7ab
memberService2 = hello.core.member.MemberServiceImpl@2de23121
```

- 스프링 없는 순수한 DI 컨테이너인 AppConfig는 요청을 할 때마다 새로운 객체를 생성한다.
- 고객 트래픽이 초당 100이 나오면 초당 100개 객체가 생성되고 소멸된다. → 메모리 낭비가 심하다.
- 해결방안으로는 해당 객체가 딱 1개만 생성되고, 공유하도록 설계해야 한다. → 싱글톤 패턴

## 싱글톤 패턴

- 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴
- 똑같은 타입의 객체 인스턴스를 2개 이상 생성하지 못하도록 막아야 한다.
    - private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 해야한다.

### 싱글톤을 사용한 테스트

```java
package hello.core.singleton;

public class SingletonSevice {

    // 1. static 영역에 객체를 딱 1개만 생성한다.
    private static final SingletonSevice instance = new SingletonSevice();

    // 2. public으로 열어서 객체 인스턴스가 필요하면 이 static 메소드를 통해서만 조회하도록 허용한다.
    public static SingletonSevice getInstance() {
        return instance;
    }

    // 3. 생성자를 private으로 선언해서 외부에서 new 키워드를 사용한 객체 생성을 못하게 막는다.
    private SingletonSevice() {

    }

    public void logic() {
        System.out.println("싱글톤 객체 로직 호출");
    }
}
```

1. static 영역에 객체를 딱 1개만 생성한다.
2. public으로 열어서 객체 인스턴스가 필요하면 이 static 메소드를 통해서만 조회하도록 허용한다.
3. 생성자를 private으로 선언해서 외부에서 new 키워드를 사용한 객체 생성을 못하게 막는다.

```java
	  @Test
    @DisplayName("싱글톤 패턴을 적용한 객체 사용")
    void singletonServiceTest() {
        SingletonSevice singletonSevice1 = SingletonSevice.getInstance();
        SingletonSevice singletonSevice2 = SingletonSevice.getInstance();

        System.out.println("singletonService1 = " + singletonSevice1);
        System.out.println("singletonService2 = " + singletonSevice2);

        assertThat(singletonSevice1).isSameAs(singletonSevice2);
        // same는 ==과 같다.
        // equal는 .isEquals과 같다.

// 결과
singletonService1 = hello.core.singleton.SingletonSevice@3d34d211
singletonService2 = hello.core.singleton.SingletonSevice@3d34d211
```

- 호출할 때마다 같은 객체 인스턴스가 반환되는 것을 확인할 수 있다.
- 싱글톤 패턴을 적용하면 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 사용할 수 있다.

## 싱글톤 문제점

- 싱글톤 패턴을 구현하는 코드 자체가 만이 들어간다.
- 의존관계상 클라이언트가 구체 클래스에 의존한다 → DIP를 위반한다.
- 클라이언트가 구체 클래스에 의존해서 OCP원칙을 위반할 가능성이 높다.
- 테스트하기 어렵다.
- 내부 속성을 변경하거나 초기화하기 어렵다.
- private 생성자로 자식 클래스를 만들기 어렵다.
- 유연성이 떨어진다.
- 안티패턴이라고 불리기도 한다.

## 싱글톤 컨테이너

- 스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 이렇게 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라고 한다.
- 스프링 컨테이너의 이런 기능 덕분에 싱글톤 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수 있다.
    - 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다.
    - DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤을 사용할 수 있다.

```java
		@Test
    @DisplayName("스프링 컨테이너와 싱글톤")
    void springContainer() {

        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService1 = ac.getBean("memberService", MemberService.class);
        MemberService memberService2 = ac.getBean("memberService", MemberService.class);

        // 참조값이 같은 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        // memeberService == memberService2
        assertThat(memberService1).isSameAs(memberService2);

// 결과
memberService1 = hello.core.member.MemberServiceImpl@2101b44a
memberService2 = hello.core.member.MemberServiceImpl@2101b44a
```

- 스프링 컨테이너 덕분에 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 재사용할 수 있다.
- 참고 : 스프링의 기본 빈 등록 방식은 싱글톤이지만, 싱글톤 방식만 지원하는 것은 아니다. 요청할 때마다 새로운 객체를 생성해서 반환하는 기능도 제공한다.

# @Configuration과 싱글톤

```java
public class MemberServiceImpl implements MemberService {
    private final MemberRepository memberRepository;
		
		//테스트 용도
		public MemberRepository getMemberRepository() {
          return memberRepository;
	  }
}

public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
	
	//테스트 용도
	public MemberRepository getMemberRepository() {
          return memberRepository;
  }
}
```

```java
import hello.core.AppConfig;
import hello.core.member.MemberRepository;
import hello.core.member.MemberServiceImpl;
import hello.core.order.OrderServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ConfigurationSingletonTest {

    @Test
    void configrationTest() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        System.out.println("memberService -> memberRepository = " + memberRepository1);
        System.out.println("orderService -> memberRepository = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        Assertions.assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
        Assertions.assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
    }
}
```

```java
memberService -> memberRepository = hello.core.member.MemoryMemberRepository@52d645b1
orderService -> memberRepository = hello.core.member.MemoryMemberRepository@52d645b1
memberRepository = hello.core.member.MemoryMemberRepository@52d645b1
```

- memberRepository 인스턴스 모두 같은 인스턴스가 공유되어 사용하고 있다.
- AppConfig의 코드를 보면 분명히 각각 2번 `new MemoryMemberRepositroy` 를 호출해서 다른 인스턴스가 생성되야할텐데 어찌 된 일일까?

```java
import hello.core.discount.DiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Configuration은 애플리케이션의 구성 정보를 담당한다.
 * 각 메서드에 @Bean을 입력하면 스프링 컨테이너에 저장된다.
 */
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        System.out.println("call AppConfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemoryMemberRepository memberRepository() {
        System.out.println("call AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        System.out.println("call AppConfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public DiscountPolicy discountPolicy() {
//        return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

- 위 코드대로라면
    - `new memberService()` 호출
    - memberService() 로직에서  `new memberRepository()` 호출
    - `new orderService()` 호출
    - orderService() 로직에서 `new memberRepository()` 호출
    - `new memberRepository()` 호출
- 하지만 출력 결과는 모두 1번만 호출된다.

```java
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
```

## @Configuration과 바이트코드 조작

스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다. 바로 `@Configuration` 이다.

```java
@Test
    void configurationDeep() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        AppConfig bean = ac.getBean(AppConfig.class);

        System.out.println("bean = " + bean.getClass());
    }
```

```java
bean = class hello.core.AppConfig$$EnhancerBySpringCGLIB$$9a78c499
```

- `AnnotationConfigApplicationContext` 에 매개변수로 넘긴 값은 스프링 빈으로 등록된다. 그래서 AppConfig도 스프링 빈이다.
- 보통 순수한 클래스라면 `class hello.core.AppConfig` 처럼 나와야한다.
- 뒤에 CGLIB가 붙으면서 복잡해진 이유는 스프링이 CGLIB라는 바이트코드 조작 라이브러리르 사용해서 AppConfig 클래스를 상속받은 임의의 클래스를 만드록, 다른 클래스를 스프링 빈으로 등록했기 때문이다.
- 이 임의의 클래스가 싱글톤을 보장해준다.
- @Bean이 붙은 메소드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 아니라면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.

### @Configuration을 적용하지 않고 @Bean만 적용하면 어떻게 될까?

```java
bean = class hello.core.AppConfig
```

- 순수한 AppConfig로 스프링 빈에 등록된다.

```java
call AppConfig.memberService
call AppConfig.memberRepository    
call AppConfig.orderService
call AppConfig.memberRepository
call AppConfig.memberRepository
```

- MemberRepository가 3번 호출된다.
- 당연히 모두 각각 다른 인스턴스를 가지고 있다.

## 정리

- @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
    - `memberRepository()` 처럼 의존관계 주입이 필요해서 메소드를 직접 호출할 때 싱글톤을 보장하지 않는다.
- 크게 고민할 것 없이 스프링 설정 정보는 항상 `@Configuration` 을 사용하자