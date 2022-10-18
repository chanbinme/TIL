* [목차](#목차)
* [Component Scan](#component-scan)
    + [basePackage](#basepackage)
    + [Component Scan 기본 대상](#component-scan-기본-대상)
        + [필터](#필터)
    + [중복 등록과 충돌](#중복-등록과-충돌)
        + [자동 빈 등록 vs 자동 빈 등록](#자동-빈-등록-vs-자동-빈-등록)
        + [수동 빈 등록 vs 자동 빈 등록](#수동-빈-등록-vs-자동-빈-등록)    
        
# Component Scan

> 컴포넌트 스캔은 설정 정보 없이 자동으로 스프링 빈을 등록하는 스프링이 제공하는 기능이다.
> 
- 설정 정보에 등록할 스프링 빈들을 직접 작성할 수 있지만, 이렇게 수작업으로 등록하게 되면 설정 정보가 커지고 누락되는 등 다양한 문제가 발생할 수 있다.
- `@ComponentScan` 은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록해주기 때문에 설정 정보에 붙여주면 된다.
    - 의존 관계를 자동으로 주입해주는 `@Autowired` 기능도 제공한다.
- XML 방식의 Component Scan

```xml
<beans>
	<context:component-scan base-package="com.acme"/>
</beans>
```

## basePackage

> 탐색할 때 패키지의 시작 위치를 지정하고, 해당 패키지부터 하위 패키지를 모두 탐색한다.
> 
- `@ComponentScan()` 의 매개변수로 basePackages=””를 줄 수 있다.
- 별도로 지정하지 않으면, `@ComponentScan` 이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.
- 스프링 부트를 사용하면 `@SpringBootApplication` 를 이 프로젝트 시작 루트 위치에 두는 것이 좋다.
    - `@SpringBootApplication` 에 `@ComponentScan` 이 들어있다.

## Component Scan 기본 대상

- `@Component` : 컴포넌트 스캔에서 사용된다.
- `@Controller` & `@RestController` : 스프링MVC 및 REST 전용 컨트롤러에 사용된다.
- `@Service` : 스프링 비즈니스 로직에서 사용된다.
    - 특별한 처리를 하지 않는다.
    - 개발자들이 핵심 비즈니스 로직이 여기에 있다는 비즈니스 계층을 인식하는데 도움이 된다.
- `@Repository` : 스프링 데이터 접근 계층에서 사용된다.
    - 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다.
- `@Configuration` : 스프링 설정 정보에서 사용된다.
    - 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다.

### 필터

- includeFilters : 컴포넌트 스캔 대상을 추가로 지정한다.
- excludeFilters : 컴포넌트 스캔에서 제외할 대상을 지정한다.
- FilterType 옵션
    - ANNOTATION : 기본값, 애너테이션으로 인식해서 동작한다.
    - ASSIGNALBLE_TYPE : 지정한 타입과 자식 타입을 인식해서 동작한다.
    - ASPECT : AspectJ패턴을 사용한다.
    - REGEX : 정규 표현식을 사용한다.
    - CUSTOM : `TypeFilter` 라는 인터페이스를 구현해서 처리한다.

## 중복 등록과 충돌

컴포넌트 스캔에서 같은 빈 이름을 등록하면 어떻게 되는지 알아보자

### 자동 빈 등록 vs 자동 빈 등록

- 컴포넌트 스캔에 의해 자동으로 스프링 빈이 등록될 때, 이름이 같은 경우 스프링이 오류를 발생시킨다.
    - `ConflictingBeanDefinitionException` 예외 발생

### 수동 빈 등록 vs 자동 빈 등록

```java
@Component
public class MemoryMemberRepository implements MemberRepository {}

@Configuration
@ComponentScan(
          excludeFilters = @Filter(type = FilterType.ANNOTATION, classes =
Configuration.class)
)
public class AutoAppConfig {
			/* 빈 이름을 MemoryMemberRepository와 동일하게 변경 */
      @Bean(name = "memoryMemberRepository")
      public MemberRepository memberRepository() {
          return new MemoryMemberRepository();
      }
}
```

- 자동 빈과 수동 빈의 이름이 겹칠 경우 수동 빈이 자동 빈을 오버라이딩 해버린다.
- **수동 빈 등록시 남는 로그**
    
    ```java
    Overriding bean definition for bean 'memoryMemberRepository' with a different
      definition: replacing
    ```
    
- 수동 빈이 자동 빈을 오버라이딩 해버린 경우 개발자가 의도한 상황이 아니라면 정말 잡기 어려운 버그가 만들어진다. 애매한 버그가 가장 잡기 어렵다고…
- 이러한 문제가 잦자 스프링 부트에서는 수동 빈 등록과 자동 빈 등록이 충돌나면 오류가 발생하도록 기본 값을 바꾸었다. 이 **오류는 스프링 부트인 `CoreApplication` 을 실행해보면 볼 수 있다.**
- **수동 빈 등록, 자동 빈 등록 오류 시 발생하는 스프링 부트 에러**
    
    ```java
    Consider renaming one of the beans or enabling overriding by setting
    spring.main.allow-bean-definition-overriding=true
    ```