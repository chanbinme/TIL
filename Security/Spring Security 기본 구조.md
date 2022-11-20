# Spring Security의 기본 구조(SSR)

- SSR(Server Side Rendering) 방식의 애플리케이션은 클라이언트에게 전송하는 HTML 코드까지 포함하고 있다.
- HTML 뷰를 구성하기 위해 타임리프(Thymeleaf)라는 템플릿 엔진을 사용하고 있다.
- 타임리프로 구성된 HTML 템플릿 파일 경로: `src/main/resources/templates`

## Hello Spring Security 샘플 애플리케이션의 구조

### 홈 화면

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9b3a10e3-344a-4128-987b-ac6e151ead6c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140227Z&X-Amz-Expires=86400&X-Amz-Signature=20743774e3daadb396645ece39286fa75904cf9cfe8dee2b6e075a830b29e18d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 회원 가입 화면

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7893032a-cc7f-4396-a42d-269afdcd6b40/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140239Z&X-Amz-Expires=86400&X-Amz-Signature=ffc08fe53f3001989e6889fcc21b4086416ad251a22ba8831b5e04d5e0fc5a85&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 로그인 화면

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2080fd06-8b8e-4c6e-b824-c5da3f668d3a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140251Z&X-Amz-Expires=86400&X-Amz-Signature=acc31261e30e47b0f8d48521bb17df8370809a3a22e9a25ac78f8a04383d8621&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 커피 보기 화면

- Spring Security를 적용했을 때, 모든 사용자들이 접근 가능한 페이지로 설정

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a4917a90-a64c-4682-bff9-8b3c1fd24df1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140316Z&X-Amz-Expires=86400&X-Amz-Signature=8e3ad57e408af6251284f12f1c4370b21303f063f2cf90795064865270617421&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 전체 주문 목록 보기 화면

- Spring Security를 적용했을 때, 관리자만 접근 가능한 페이지로 설정

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7768d961-c6ba-47f8-ac44-d9518e3b0362/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140332Z&X-Amz-Expires=86400&X-Amz-Signature=ece8a584c14628457689f5a3fcc035866f8fc70c98ae97d213be768d37a415f4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 마이페이지 화면

- Spring Security를 적용했을 때, 일반 사용잠나 접근 가능한 페이지로 설정

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a572424a-d983-4788-8405-f9a773286a4a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140346Z&X-Amz-Expires=86400&X-Amz-Signature=30142098e45068a0de0f80b469a97297eb2ba3478a1f30fb31d9761149d488f5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 회원 가입 HTML 폼

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="layouts/common-layout">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Hello Spring Security Coffee Shop</title>
</head>
<body>
    <hr />
    <div class="container" layout:fragment="content">
        <!-- (1) 회원 가입 폼 -->
        <form action="/members/register" method="post">
            <div class="row">
                <div class="col-xs-2">
                    <input type="text" name="fullName" class="form-control" placeholder="User Name"/>
                </div>
            </div>
            <div class="row" style="margin-top: 20px">
                <div class="col-xs-2">
                    <input type="email" name="email" class="form-control" placeholder="Email"/>
                </div>
            </div>
            <div class="row" style="margin-top: 20px">
                <div class="col-xs-2">
                    <input type="password" name="password" class="form-control" placeholder="Password"/>
                </div>
            </div>

            <button class="btn btn-outline-secondary" style="margin-top: 20px">회원 가입</button>
        </form>
    </div>
</body>
</html>
```

## Spring Security 적용

### 의존 라이브러리 추가(build.gradle)

```groovy
dependencies {
	...
	implementation 'org.springframework.boot:spring-boot-starter-security'   // (1)
  ...
}
```

- Spring Security의 의존 라이브러를 추가한 후 애플리케이션을 실행해보면 Spring Security가 내부적으로 제공해주는 디폴트 로그인 화면을 볼 수 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/abb68a41-0ea9-4d45-bbc2-60bbc707374b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140358Z&X-Amz-Expires=86400&X-Amz-Signature=aacd56fb7d8dd70ecf1efff05cc437995338e0a492cf81391cbd2f34bc658074&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 애플리케이션을 매번 실행할 때마다 패스워드가 바뀐다.
- Spring Security에서 제공하는 디폴트 인증 정보만으로 로그인을 하는 것은 사실상 불가능하다.
- Spring Security Configuration을 적용해서 우리가 원하는 인증 방식과 웹 페이지에 대한 접근 권한을 설정할 수 있다.

### SecurityConfiguration

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManage

// 여기에 Spring Security의 설정을 진행한다. 
@Configuration
public class SecurityConfiguration {

		...

    @Bean
    public UserDetailsManager userDetailsService() {

        UserDetails userDetails =
                User.withDefaultPasswordEncoder()    
                        .username("kevin@gmail.com") 
                        .password("1111")           
                        .roles("USER")               
                        .build();

				UserDetails admin =
				        User.withDefaultPasswordEncoder()
				                 .username("admin@gmail.com")
				                 .password("2222")
				                 .roles("ADMIN")
				                 .build();

        return new InMemoryUserDetailsManager(userDetails);
    }
}
```

- `UserDetails` : 사용자의 핵심 정보를 포함하는 인터페이스. 구현체인 `User` 클래스를 이용해서 사용자의 인증 정보를 생성한다.
- `withDefaultPasswordEncoder()` : 디폴트 패스워드 인코더로 사용자 패스워드를 암호화 한다. 해당 메서드는 Production 환경에서 사용하지 않는 것이 좋다.
- `username()` : 사용자의 username을 설정한다. 여기서 username은 사람의 이름이 아닌 고유한 사용자를 식별하는 사용자 아이디 같은 값이다. 여기서는 이메일을 username으로 지정했다.
- `password()` : 사용자의 password를 설정한다. 파라미터로 지정한 값은 `withDefaultPasswordEncoder()` 로 인해 암호화 된다.
- `roles()` : 사용자의 Role. 즉, 역할을 지정하느 메서드이다.
- `UserDetailsManager` : Spring Security에서 제공하는 인터페이스. 핵심 정보를 포함한 UserDetails를 관리한다.
- 여기서는 메모리 상에서 관리하므로 `InMemoryDetailManager` 라는 구현체를 사용했다.
- `new InMemoryUserDetailsManager(userDetails)` 를 통해 `UserDetailsManager` 객체를 Bean으로 등록하면 Spring에서는 해당 Bean이 가지고 있는 인증 정보가 클라이언트의 요청으로 넘어올 경우 정상적인 인증 프로세스를 수행한다.
- InMmory 방식은 테스트 환경 또는 데모 환경에서만 유용하게 사용할 수 있는 방식이다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

// 여기에 Spring Security의 설정을 진행한다. 
@Configuration
public class SecurityConfiguration {

		// HttpSecurity를 통해 HTTP 요청에 대한 보안 설정을 구성한다.
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()                 
            .formLogin()                      
            .loginPage("/auths/login-form")   
            .loginProcessingUrl("/process_login")    
            .failureUrl("/auths/login-form?error")   
            .and()                                  
            .authorizeHttpRequests()                
            .anyRequest()                            
            .permitAll();                           

        return http.build();
    }

		...
}
```

- HttpSecurity를 파라미터로 가지고, SecurityFilterChain을 리턴하는 형태의 메서드를 정의하면 HTTP 보안 설정을 구성할 수 있다.
- HttpSecurity는 HTTP 요청에 대한 보안 설정을 구성하기 위한 핵심 클래스이다.
- `csrf().disable()` : CSRF(Cross-Site Request Forgery) 공격에 대한 Spring Security의 설정을 비활성화한다. 기본값으로 설정하면 csrf() 공격을 방지하기 위해 클라이언트로부터 CSRF Token을 수신 후, 검증한다.
    
    지금은 로컬 환경에서 진행하기 때문에 설정을 해지했다. 설정을 하지 않으면 403 에러가 발생해 정상적인 접속이 불가능하다.
    
- `formLogin()` : 기본적인 인증 방법을 폼 로그인 방식으로 지정한다.
- `loginPage("/auths/login-form")` : 해당 경로에 미리 만들어둔 커스텀 로그인 페이지를 사용하도록 설정한다.
    
    `"/auths/login-form"` 은 AuthController의 `loginForm()` 핸들러 메서드에 요청을 전송하는 요청 URL이다. 
    
    ```java
    @Controller
    @RequestMapping("/auths")
    public class AuthController {
    
        @GetMapping("/login-form")
        public String loginForm() {
            return "login";
        }
    		
    		...
    }
    ```
    
- `loginProcessingUrl("/process_login")` : 로그인 인증 요청을 수행할 요청 URL을 지정하는 메서드.
    
    `"/process_login"` 은 login.html에서 form 태그의 action 속성에 지정한 URL과 동일하다.
    
    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org"
          xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
          layout:decorate="layouts/common-layout">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <title>Hello Spring Security Coffee Shop</title>
    </head>
    <body>
        <div layout:fragment="content">
            <form action="**/process_login**" method="post">
                <p><input type="email" name="username" placeholder="Email" /></p>
                <p><input type="password" name="password" placeholder="Password" /></p>
                <p><button>로그인</button></p>
            </form>
            <a href="/members/register">회원가입</a>
        </div>
    </body>
    </html>
    ```
    
    - `<form action="**/process_login**" method="post">` 에서 `/process_login` URL이 지정되어 있다.
    - `/process_login` URL로 요청을 전송하면 Spring Security에서 내부적으로 인증 프로세스를 진해한다.
    - 결국 login.html 같은 커스텀 로그인 페이지를 통해 Spring Security가 로그인 인증 처리를 하기 위한 요청 URL을 지정한 것이다.
- `failureUrl("/auths/login-form?error")` : 로그인 인증에 실패할 경우 어떤 화면으로 리다이렉트 할 것인가를 지정하는 메서드
    
    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org"
          xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
          layout:decorate="layouts/common-layout">
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
            <title>Hello Spring Security Coffee Shop</title>
        </head>
        <body>
            <div class="container" layout:fragment="content">
                <form action="/process_login" method="post">
                    <!-- (1) 로그인 실패에 대한 메시지 표시 -->
                    <div class="row alert alert-danger center" role="alert" th:if="${param.error != null}">
                        <div>로그인 인증에 실패했습니다.</div>
                    </div>
                    <div class="row">
                        <div class="col-xs-2">
                            <input type="email" name="username"  class="form-control" placeholder="Email" />
                        </div>
                    </div>
                    <div class="row" style="margin-top: 20px">
                        <div class="col-xs-2">
                            <input type="password" name="password"  class="form-control" placeholder="Password" />
                        </div>
                    </div>
    
                    <button class="btn btn-outline-secondary" style="margin-top: 20px">로그인</button>
                </form>
                <div style="margin-top: 20px">
                    <a href="/members/register">회원가입</a>
                </div>
            </div>
        </body>
    </html>
    ```
    
    - 로그인 인증에 실패할 경우, 인증에 실패했다는 메시지를 표시하기 위한 로직을 추가한 login.html 코드
    - `${param.error}` : `failureUrl("/auths/login-form?error")` 의 `?error` 부분에 해당하는 쿼리 파라미터. 로그인 인증 실패 메시지 표시 여부를 결정한다.
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/da9f8ea8-bef6-4ae8-bbf8-cce04d3b8652/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140417Z&X-Amz-Expires=86400&X-Amz-Signature=7e6c147e75d849340c7629e387b73fc6d9eb50a7c7c348136f476f847adafadc&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
    
- `and()` : Spring Security 보안 설정을 메서드 체인 형태로 구성할 수 있는 메서드
- `authorizeHttpRequests()` : 클라이언트의 요청이 들어오면 접근 권한을 확인하겠다고 정의하는 메서드
- `anyRequest().permitAll()` : 클라이언트의 모든 요청에 대해 접근을 허용

### Spring Security Configuration(request URI에 접근 권한 부여)

- `.authorizeHttpRequests().anyRequest().permitAll()` 설정을 통해 로그인 인증에 성공할 경우, 모든 화면에 접근할 수 있도록 했던 부분을 사용자의 Role 별로 Request URI에 접근 권한이 부여되도록 수정해보자

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfiguration {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .formLogin()
            .loginPage("/auths/login-form")
            .loginProcessingUrl("/process_login")
            .failureUrl("/auths/login-form?error")
            .and()
            .exceptionHandling().accessDeniedPage("/auths/access-denied")   // (1)
            .and()
            .authorizeHttpRequests(authorize -> authorize                  // (2)
                    .antMatchers("/orders/**").hasRole("ADMIN")        // (2-1)
                    .antMatchers("/members/my-page").hasRole("USER")   // (2-2)
                    .antMatchers("⁄**").permitAll()                    // (2-3)
            );
        return http.build();
    }

    ...
}
```

- `exceptionHandling().accessDeniedPage("/auths/access-denied")` : 권한이 없는 사용자가 특정 request URI에 접근할 경우 발생하는 `403(Forbidden)` 에러를 처리하기 위한 페이지를 설정

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/27558814-a86c-4df6-b83f-bedc393bf1c0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140436Z&X-Amz-Expires=86400&X-Amz-Signature=aef9d4a8a49ee5a8b8bf4a027bd8b06b7b2483f4f1f2e639471072701293a945&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `exceptionHandling()` : Exception을 처리하는 기능을 하며, `ExceptionHandlingConfigurer` 객체를 통해 구체적인 Exception을 처리할 수 있다.
- `accessDeniedPage()` : 403에러 발생 시, 파라미터로 지정한 URL로 리다이렉트 되도록 해준다.
- `authorizeHttpRequests()` : 람다 표현식을 통해 request URI에 대한 접근 권한을 부여할 수 있다.
- `antMatchers()` : ant라는 빌드 툴에서 사용되는 `PathPattern` 을 이용해서 매치되는 URL을 표현한다.
- `.antMatchers("/orders/**").hasRole("ADMIN")` : ADMIN Role을 부여 받은 사용자만 `/orders` 로 시작하는 모든 URL에 접근할 수 있다는 의미
- `/orders/**` : `/**` 는 `/orders` 로 시작하는 모든 하위 URL을 포함한다.
    - `/orders/1`, `/orders/1/coffees` , `/orders/1/coffees/1` 같은 모든 하위 URL을 포함한다.
    - 만약 `/orders/*` 로 지정했다면 `/orders/1` 과 같이 하위 URL의 depth가 1인 URL만 포함된다.
- `antMatchers("/members/my-page").hasRole("USER")` : USER Role을 부여 받은 사용자만 `/members/my-page` URL에 접근할 수 있다는 의미
- `.antMatchers("/**").permitAll()` : 위에서 먼저 지정한 URL 이외의 나머지 모든 URL은 Role에 상관없이 접근이 가능함을 의미
- `antMatchers()`를 이용한 접근 권한 부여 시, 주의 사항
    
    ```java
    .authorizeHttpRequests(authorize -> authorize
                        .antMatchers("⁄**").permitAll() // 이 표현식이 제일 앞에 오면?
                        .antMatchers("/orders/**").hasRole("ADMIN")
                        .antMatchers("/members/my-page").hasRole("USER")
                );
    ```
    
    - 위 코드처럼 작성할 경우 사용자의 Role과 무관하게 모든 request URL에 접근이 가능하게 된다.
    - 더 구체적인 URL 경로부터 접근 권한을 부여한 다음 덜 구체적인 URL 경로에 접근 권한을 부여해야 한다.

### 로그인 한 사용자 아이디 표시 및 사용자 로그아웃

- 어떤 사용자가 로그인 했는지 알 수 있어야 한다.
- 로그인 한 사용자가 로그아웃 할 수 있어야 한다.
- 마이 페이지 링크는 로그인 한 사용자에게만 보여야 한다.

```html
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"> <!-- (1) -->
    <body>
        <div align="right" th:fragment="header">
            <a href="/members/register" class="text-decoration-none">회원가입</a> |
            <span sec:authorize="isAuthenticated()"> <!-- (2) -->
                <span sec:authorize="hasRole('USER')">  <!-- (3) -->
                    <a href="/members/my-page" class="text-decoration-none">마이페이지</a> |
                </span>
                <a href="/logout" class="text-decoration-none">로그아웃</a>  <!-- (4) -->
                <span th:text="${#authentication.name}">홍길동</span>님  <!-- (5) -->
            </span>
            
            <span sec:authorize="!isAuthenticated()"> <!-- (6) -->
                <a href="/auths/login-form" class="text-decoration-none">로그인</a>
            </span>
        </div>
    </body>
</html>
```

- `xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"` : 타임리프 기반의 HTML 템플릿에서 사용자의 인증 정보나 권한 정보를 이용해 어떤 로직을 처리하기 위해 `sec` 태그를 사용하기 위한 XML 네임스페이스를 지정한다.
- `<span sec:authorize="isAuthenticated()">` : 현재 페이지에 접근한 사용자가 인증에 성공한 사용자인지를 체크한다.
    
    즉, `isAuthenticated()` 의 값이 `true` 이면 태그 하위에 포함된 컨텐츠를 화면에 표시한다.
    
- `<span sec:authorize="hasRole('USER')">` : 마이페이지는 ADMIN Role을 가진 사용자에게 필요없기 때문에 USER Role을 가진 사용자에게만 표시되도록 지정.
- `<a href="/logout" class="text-decoration-none">로그아웃</a>` : `isAuthenticated()` 의 값이 `true` 라는 의미는 이미 로그인한 사용자라는 의미이므로 로그인 대신 로그아웃 메뉴를 표시
    
    `href="/logout"` 에서 `"/logout"` URL은 SecurityConfiguration 클래스에서 설정한 값과 같아야 한다.
    
- `th:text="${#[authentication.name](http://authentication.name/)}"` : 로그인 사용자의 username을 표시하고 있다. 이 곳에는 로그인할 때 사용한 username이 표시된다.
- `<span sec:authorize="!isAuthenticated()">` : 로그인한 사용자가 아니라면 로그인 버튼이 표시되도록 한다.

### sec 태그를 사용하기 의존성 주입(build.gradle)

```groovy
dependencies {
	...
	implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity5'
}
```

### SecurityConfiguration(로그아웃 기능 추가)

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfiguration {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .formLogin()
            .loginPage("/auths/login-form")
            .loginProcessingUrl("/process_login")
            .failureUrl("/auths/login-form?error")
            .and()
            .logout()                        // (1)
            .logoutUrl("/logout")            // (2)
            .logoutSuccessUrl("/")  // (3)
            .and()
            .exceptionHandling().accessDeniedPage("/auths/access-denied")
            .and()
            .authorizeHttpRequests(authorize -> authorize
                    .antMatchers("/orders/**").hasRole("ADMIN")
                    .antMatchers("/members/my-page").hasRole("USER")
                    .antMatchers("⁄**").permitAll()
            );
        return http.build();
    }
    
    ...
    ...
}
```

- `logout()` : 로그아웃 설정을 위한 `LogoutConfigurer` 를 리턴한다. 로그아웃에 대한 추가 설정을 위해서는 `logout()` 을 먼저 호출해야 한다.
- `logoutUrl("/logout")` : 사용자가 로그아웃을 수행하기 위한 request URL을 지정한다. 여기서 설정한 URL은 header.html의 로그아웃 메뉴에 지정한 `href="/logout"` 과 동일해야 한다.
- `logoutSuccessUrl("/")` : 로그아웃을 성공적으로 수행한 이후 리다이렉트 할 URL을 지정한다.

## 회원가입 폼을 통한 InMemory User 등록

- 회원가입 폼을 통해 InMemory User를 등록하기 위한 작업 순서는 다음과 같다.
    - PasswordEncoder Bean 등록
    - MemberService Bean 등록을 위한 JavaConfiguration 구성
    - InMemoryMemberService 클래스 구현

### PasswordEncoder Bean 등록

- 회원 가입 폼에서 전달 받은 패스워드는 InMemory User로 등록되기 전에 암호화 되어야 한다.
- PasswordEncoder는 다양한 암호화 방식을 제공하며, Spring Security에서 지원하는 PasswordEncoder의 디폴트 암호화 알고리즘은 bcrypt이다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.crypto.factory.PasswordEncoderFactories;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.provisioning.UserDetailsManager;

@Configuration
public class SecurityConfiguration {
    ...
    ...

    @Bean
    public UserDetailsManager userDetailsService() {
        UserDetails user =
                User.withDefaultPasswordEncoder()
                        .username("kevin@gmail.com")
                        .password("1111")
                        .roles("USER")
                        .build();

        UserDetails admin =
                User.withDefaultPasswordEncoder()
                        .username("admin@gmail.com")
                        .password("2222")
                        .roles("ADMIN")
                        .build();

        return new InMemoryUserDetailsManager(user, admin);
    }

    // (1)
    @Bean
    public PasswordEncoder passwordEncoder() {
        return PasswordEncoderFactories.createDelegatingPasswordEncoder();  // (1-1)
    }
}
```

- `PasswordEncoderFactories.createDelegatingPasswordEncoder();` : `DelegatingPasswordEncoder` 를 먼저 생성하는데, 이 `DelegatingPasswordEncoder` 가 실질적으로 PasswordEncoder 구현 객체를 생성해 준다.

### JavaConfiguration에 등록하기 위한 MemberService Bean

```java
public interface MemberService {
		Member createMember(Member member)
}
```

### 데이터베이스에 User를 등록하기 위한 DBMemberService 클래스

```java
package com.codestates.member;

import org.springframework.transaction.annotation.Transactional;

@Transactional
public class DBMemberService implements MemberService {
    public Member createMember(Member member) {
         return null;
    }
}
```

### MemberService Bean 등록을 위한 JavaConfiguration 구성

```java
import com.codestates.member.InMemoryMemberService;
import com.codestates.member.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.UserDetailsManager;

@Configuration
public class JavaConfiguration {
    // (1)
    @Bean
    public MemberService inMemoryMemberService(UserDetailsManager userDetailsManager, 
                                               PasswordEncoder passwordEncoder) {
        return new InMemoryMemberService(userDetailsManager, passwordEncoder);
    }
}
```

- MemberService 인터페이스의 구현체인 InMemoryMemberService 클래스의 Bean 객체를 생성한다.
- InMemoryMemberService 클래스는 데이터베이스 연동 없이 메모리에 Spring Security의 User를 등록해야하므로 UserDetailsManager 객체가 필요하다.
- User 등록 시, 패스워드를 암호화 한 후에 등록해야 하므로 Spring Security에서 제공하는 `PasswordEncoder` 객체가 필요하다.

### InMemory User 등록을 위한 InMemoryMemberService 클래스

```java
import com.codestates.auth.utils.AuthorityUtils;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.UserDetailsManager;

import java.util.List;

public class InMemoryMemberService implements MemberService {  // (1)
    private final UserDetailsManager userDetailsManager;
    private final PasswordEncoder passwordEncoder;

    // (2)
    public InMemoryMemberService(UserDetailsManager userDetailsManager, PasswordEncoder passwordEncoder) {
        this.userDetailsManager = userDetailsManager;
        this.passwordEncoder = passwordEncoder;
    }

    public Member createMember(Member member) {
        // (3)
        List<GrantedAuthority> authorities = createAuthorities(Member.MemberRole.ROLE_USER.name());

        // (4)
        String encryptedPassword = passwordEncoder.encode(member.getPassword());

        // (5)
        UserDetails userDetails = new User(member.getEmail(), encryptedPassword, authorities);

        // (6)
        userDetailsManager.createUser(userDetails);

        return member;
    }

    private List<GrantedAuthority> createAuthorities(String... roles) {
        // (3-1)
        return Arrays.stream(roles)
                .map(role -> new SimpleGrantedAuthority(role))
                .collect(Collectors.toList());
    }
}
```

- `UserDetailsManager` 와 `PasswordEncoder` 를 DI 받는다.
    - `UserDetailsManager` : Spring Security의 User를 관리하는 관리자 역할을 한다.
    - `UserDetailsManager` : DI 받은 `UserDetailsManager` 인터페이스의 하위 타입은 `InMemoryUserDetailManager` 이다.
    - `PasswordEncoder` : Spring Security User를 등록할 때 패스워드를 암호화 해주는 클래스
    - Spring Security 5에서는 InMemory User의 경우에도 패스워드의 암호화가 필수이다. DI 받은 `PasswordEncoder` 를 이용해 User의 패스워드를 암호화 해 주어야 한다.
- Spring Security에서 User를 등록하기 위해서는 해당 User의 권한(Authority)를 지정해줘야 한다.
- `createAuthorities(Member.MemberRole.ROLE_USER.name());` : User의 권한 목록을 `List<GrantedAuthority>` 로 생성한다.
- `SimpleGrantedAuthority` 를 사용해 Role 베이스 형태의 권한을 지정할 때 `'Roll_' + 권한명` 형태로 지정해줘야 한다. 그렇지 않을 경우 적절한 권한 매핑이 이루어지지 않는다.
    
    ```java
    Arrays.stream(roles)
                    .map(role -> new SimpleGrantedAuthority(role))
                    .collect(Collectors.toList());
    ```
    
    - 파라미터로 User의 Role을 전달하면서 `SimpleGrantedAuthority` 객체를 생성한 후, `List<SimpleGrantedAuthority>` 형태로 리턴해준다.
- `String encryptedPassword = passwordEncoder.encode(member.getPassword());` : `PasswordEncoder` 를 이용해 패스워드를 암호화 하고 있다. 만약 암호화 하지 않고 등록하면 에러가 발생한다.
- `UserDetails userDetails = new User(member.getEmail(), encryptedPassword, authorities);` : Spring Security User로 등록하기 위해 `UserDetails` 를 생성한다. Spring Security에서는 User 정보를 `UserDetails` 로 관리한다는 사실을 꼭 기억하자.
- `userDetailsManager.createUser(userDetails);` : User를 등록

## 데이터베이스 연동을 통한 로그인 인증

- Spring Security에서는 User의 인증 정보를 테이블에 저장하고, 테이블에 저장된 인증 정보를 이용해 인증 프로세스를 진행할 수 있는 몇 가지 방법이 존재하는데 그 중 한 가지 방법인 Custom UserDetailsService를 이용해보자
- Spring Security에서는 인증을 시도하는 주체를 User라고 부른다.
- Principal은 User의 더 구체적인 정보를 의미하며, 일반적으로 Username을 의미한다.
- Member 엔티티가 Spring Security의 User 정보를 포함한다고 보면 된다.

### SecurityConfiguration 설정 변경 및 추가

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.annotation.Order;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.crypto.factory.PasswordEncoderFactories;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.provisioning.UserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfiguration {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .headers().frameOptions().sameOrigin() // (1)
            .and()
            .csrf().disable()
            .formLogin()
            .loginPage("/auths/login-form")
            .loginProcessingUrl("/process_login")
            .failureUrl("/auths/login-form?error")
            .and()
            .logout()
            .logoutUrl("/logout")
            .logoutSuccessUrl("/")
            .and()
            .exceptionHandling().accessDeniedPage("/auths/access-denied")
            .and()
            .authorizeHttpRequests(authorize -> authorize
                    .antMatchers("/orders/**").hasRole("ADMIN")
                    .antMatchers("/members/my-page").hasRole("USER")
                    .antMatchers("⁄**").permitAll()
            );
        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return PasswordEncoderFactories.createDelegatingPasswordEncoder();
    }
}
```

- `.headers().frameOptions().sameOrigin()` : 웹 브라우저에서 H2 웹 콘솔을 정상적으로 사용하기 위한 설정이다.
- `frameOptions()` : HTML 태그 중에서 `<frame>` 이나 `<iframe>` , `<object>` 태그에서 페이지를 렌더링 할지의 여부를 결정하는 기능을 한다.
    
    Spring Security에서는 Clickjacking 공격을 막기 위해 기본적으로 `frameOptions()` 기능이 활성화 되어 있으며 기본값은 `DENY` 이다. 즉, HTML 태그를 이용한 페이지 렌더링을 허용하지 않겠다는 의미이다.
    
- `.frameOptions().sameOrigin()` : 동일 출처로부터 들어오는 request만 페이지 렌더링을 허용한다.

### JavaConfiguration의 Bean 등록 변경

```java
@Configuration
public class JavaConfiguration {

    @Bean
    public MemberService dbMemberService(MemberRepository memberRepository,
                                         PasswordEncoder passwordEncoder) {
        return new DBMemberService(memberRepository, passwordEncoder); (1-1)
```

- User의 정보를 저장하기 위해 MemberService 인터페이스의 구현 클래스를 DBMemberService로 변경
- DBMemberService는 내부에서 데이터를 데이터베이스에 저장하고, 패스워드를 암호화 해야 하므로 `MemberRepository` 와 `PasswordEncoder` 객체를 DI 해준다.

### DBMemberService 구현

```java
import com.codestates.auth.utils.HelloAuthorityUtils;
import com.codestates.exception.BusinessLogicException;
import com.codestates.exception.ExceptionCode;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.Optional;

@Transactional
public class DBMemberService implements MemberService {

    private final MemberRepository memberRepository;
    private final PasswordEncoder passwordEncoder;
    private final HelloAuthorityUtils authorityUtils;

    // 생성자를 통해 MemberRepository와 PasswordEncoder Bean 객체를 DI 받는다.
    public DBMemberService(MemberRepository memberRepository, PasswordEncoder passwordEncoder, HelloAuthorityUtils authorityUtils) {
        this.memberRepository = memberRepository;
        this.passwordEncoder = passwordEncoder;
        this.authorityUtils = authorityUtils;
    }

    public Member createMember(Member member) {
        verifyExistsEmail(member.getEmail());
        // PasswordEncoder를 이용해 패스워드를 암호화한다.
        // 패스워드는 복호화 할 일이 없기 때문에 단방향 암호화 방식으로 암호화되어야 한다.
        String encryptedPassword = passwordEncoder.encode(member.getPassword());
        // 암호화 된 패스워드를 password 필드에 다시 할당한다.
        member.setPassword(encryptedPassword);

        List<String> roles = authorityUtils.createRoles(member.getEmail());
        member.setRoles(roles);

        Member savedMember = memberRepository.save(member);
        return savedMember;
    }

    public Member findMember(String email) {
        return findVerifiedMember(email);
    }

    public Member findVerifiedMember(String email) {
        Optional<Member> optionalMember = memberRepository.findByEmail(email);

        Member findMember = optionalMember
                .orElseThrow(() ->
                        new BusinessLogicException(ExceptionCode.MEMBER_NOT_FOUND));
        return findMember;
    }

    private void verifyExistsEmail(String email) {
        Optional<Member> member = memberRepository.findByEmail(email);
        if (member.isPresent())
            throw new BusinessLogicException(ExceptionCode.MEMBER_EXISTS);
    }
}
```

- DBMemberService는 User의 인증 정보를 데이터베이스에 저장하는 역할을 한다.
- `String encryptedPassword = passwordEncoder.encode(member.getPassword());` : PasswordEncoder를 이용해 패스워드를 암호화한다. 패스워드는 복호화 할 일이 없기 때문에 단방향 암호화 방식으로 암호화되어야 한다.
- `member.setPassword(encryptedPassword);` : 암호화 된 패스워드를 password 필드에 다시 할당한다.

### Custom UserDetailsService 구현

- Custom UserDetailsService는 데이터베이스에서 조회한 User의 인증 정보를 기반으로 인증을 처리한다.
- `UserDetailsService` : Spring Security에서 제공하는 컴포넌트 중 하나이다. User 정보를 로드(load)하는 핵심 인터페이스이다. 여기서 로드의 의미는 인증에 필요한 User 정보를 어딘가에서 가지고 온다는 의미이며, 여기서 말하는 ‘어딘가’는 메모리가 될 수도 있고, DB등의 영구 저장소가 될 수도 있다.
- `InMemoryUserDetailsManager` 는 `UserDetailsManager` 인터페이스의 구현체이고, `UserDetailsManager` 는 `UserDetailService` 를 상속하는 확장 인터페이스이다.

```java
import com.codestates.auth.utils.HelloAuthorityUtils;
import com.codestates.exception.BusinessLogicException;
import com.codestates.exception.ExceptionCode;
import com.codestates.member.Member;
import com.codestates.member.MemberRepository;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;

import java.util.Collection;
import java.util.Optional;

@Component
public class HelloUserDetailsService implements UserDetailsService { 
    private final MemberRepository memberRepository;
    private final HelloAuthorityUtils authorityUtils;

    // 데이터베이스에서 User를 조회하고, 조회한 User의 권한 정보를 생성하기 위해 DI 받는다.
    public HelloUserDetailsServiceV1(MemberRepository memberRepository, HelloAuthorityUtils authorityUtils) {
        this.memberRepository = memberRepository;
        this.authorityUtils = authorityUtils;
    }

    // UserDetailsService 인터페이스를 구현하는 클래스는 loadUserByUsernames(String username)이라는 추상 메서드를 구현해야 한다.
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Member> optionalMember = memberRepository.findByEmail(username);
        Member findMember = optionalMember.orElseThrow(() -> new BusinessLogicException(ExceptionCode.MEMBER_NOT_FOUND));

        // 데이터베이스에서 조회한 회원의 이메일 정보를 이용해 Role 기반의 권한 정보(GrantedAuthority) 컬렉션을 생성한다.
        Collection<? extends GrantedAuthority> authorities = authorityUtils.createAuthorities(findMember.getEmail());

        // UserDetails 인터페이스의 구현체인 User 클래스의 객체를 통해 제공하고 있다.
        return new HelloUserDetails(findMember);
    }

	// UserDetails 인터페이스를 구현하고 Member를 상속함으로써, 데이터베이스에서 조회한 회원 정보를
	// Spring Security의 User 정보로 변화하는 과정과 User의 권한 정보를 생성하는 과정을 캡슐화 할 수 있다.
	private final class HelloUserDetails extends Member implements UserDetails { 
        
        HelloUserDetails(Member member) {
            setMemberId(member.getMemberId());
            setName(member.getName());
            setEmail(member.getEmail());
            setPassword(member.getPassword());
        }

        @Override
        public Collection<? extends GrantedAuthority> getAuthorities() {
						// createAuthorities() 메서드를 이용해 User의 권한 정보를 생성하고 있다.
            return authorityUtils.createAuthorities(this.getEmail());  
        }

        // Spring Security에서 인식할 수 있는 username을 Member의 email 주소로 채우고 있다. 
        @Override
        public String getUsername() {
            return getEmail();
        }

        @Override
        public boolean isAccountNonExpired() {
            return true;
        }

        @Override
        public boolean isAccountNonLocked() {
            return true;
        }

        @Override
        public boolean isCredentialsNonExpired() {
            return true;
        }

        @Override
        public boolean isEnabled() {
            return true;
        }
    }

}
} 
```

- 데이터베이스에서 User의 인증 정보만 Spring Security에게 넘겨주면, 인증 처리는 Spring Security가 대신해준다.
- `UserDetails` : UserDetailsService에 의해 로드되어 인증을 위해 사용되는 핵심 User 정보를 표현하는 인터페이스.
    
    Spring Security에서 보안 정보 제공을 목적으로 직접 사용되지 않고, `Authentication` 객체로 캡슐화 되어 제공된다.
    

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.stereotype.Component;
import java.util.List;

@Component
public class HelloAuthorityUtils {

		// 특정 이메일 주소에 관리자 권한을 부여
    @Value("${mail.address.admin}")
    private String adminMailAddress;

    // AuthorityUtils 클래스를 이용해서 관리자 권한 목록을 객체로 생성
    private final List<GrantedAuthority> ADMIN_ROLES = AuthorityUtils.createAuthorityList("ROLE_ADMIN", "ROLE_USER");

    // AuthorityUtils 클래스를 이용해서 일반 사용자 권한 목록을 객체로 생성
    private final List<GrantedAuthority> USER_ROLES = AuthorityUtils.createAuthorityList("ROLE_USER");
    
    public List<GrantedAuthority> createAuthorities(String email) {
        // 파라미터로 전달 받은 이메일 주소가 application.yml파일에서 가져온 관리자용 이메일 주소와 동일하다면 
				// 관리자용 권한인 ADMIN_ROLES를 리턴
        if (email.equals(adminMailAddress)) {
            return ADMIN_ROLES;
        }
        return USER_ROLES;
    }
}
```

- `@Value("${mail.address.admin}")` : application.yml에 추가한 프로퍼티를 가져오는 표현식. `@Value("${프로퍼티 경로}")` 의 표현식대로 작성하면 application.yml에 정의되어 있는 프로퍼티의 값을 클래스 내에서 사용할 수 있다.
    
    application.yml에 미리 정의한 관리자 권한을 가질 수 있는 이메일 주소를 불로오고 있다. 이 주소는 회원 등록 시, 특정 이메일 주소에 관리자 권한을 부여할 수 있는 지 여부를 결정하기 위해 사용된다.
    
- application.yml 파일에 다음과 같이 관리자 이메일 주소가 정의되어야 한다.

```yaml
...
...

mail:
  address:
    admin: admin@gmail.com
```

- 실무에서는 지금보다 관리자로서 등록하기 위한 추가적인 인증 절차가 필요하다.

## User의 Role을 DB에서 관리하기

- User의 인증 정보 같은 보안과 관련된 정보는 데이터베이스 같은 영구 저장소에 안전하게 보관한다.
- User의 권한 정보를 데이터베이스에서 관리하도록 코드를 수정하기 위해서는 다음과 같은 과정이 필요하다.
    - User의 권한 정보를 저장하기 위한 테이블 생성
    - 회원 가입 시, User의 권한 정보(Role)를 데이터베이스에 저장
    - 로그인 인증 시, User의 권한 정보를 데이터베이스에서 조회

### User의 권한 정보 테이블 생성

- User와 User의 권한 정보 간에 연관 관계를 맺어야 한다.

```java
import com.codestates.audit.Auditable;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.*;
import java.security.Principal;
import java.util.ArrayList;
import java.util.List;

@NoArgsConstructor
@Getter
@Setter
@Entity
public class Member extends Auditable implements Principal{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long memberId;

    @Column(length = 100, nullable = false)
    private String fullName;

    @Column(nullable = false, updatable = false, unique = true)
    private String email;

    @Column(length = 100, nullable = false)
    private String password;

    @Enumerated(value = EnumType.STRING)
    @Column(length = 20, nullable = false)
    private MemberStatus memberStatus = MemberStatus.MEMBER_ACTIVE;

    //  User의 권한 정보 테이블과 매핑되는 정보
    @ElementCollection(fetch = FetchType.EAGER)
    private List<String> roles = new ArrayList<>();

    public Member(String email) {
        this.email = email;
    }

    public Member(String email, String fullName, String password) {
        this.email = email;
        this.fullName= fullName;
        this.password = password;
    }

    @Override
    public String getName() {
        return getEmail();
    }

    public enum MemberStatus {
        MEMBER_ACTIVE("활동중"),
        MEMBER_SLEEP("휴면 상태"),
        MEMBER_QUIT("탈퇴 상태");

        @Getter
        private String status;

        MemberStatus(String status) {
           this.status = status;
        }
    }

    public enum MemberRole {
        ROLE_USER,
        ROLE_ADMIN
    }
}
```

- List, Set 같은 컬렉션 타입의 필드는 `@ElementCollection` 애너테이션을 추가하면 User 권한 정보와 관련된 별도의 엔티티 클래스를 생성하지 않아도 간단하게 매핑 처리가 된다.
- 한 명의 회원이 한 개 이상의 Role을 가질 수 있으므로, Member 테이블과 `MEMBER_ROLES` 테이블은 1대N 관계이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c498ad66-8f85-4f3b-9c76-a923c7e9b97b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221120T140538Z&X-Amz-Expires=86400&X-Amz-Signature=06513e907b0774cdf72b50973aa37fd661f86d0900bafc39088bbe0d34702b7b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### 회원 가입 시, User의 권한 정보(Role)를 데이터베이스에 저장

```java
@Transactional
public class DBMemberService implements MemberService {
    ...
    ...
  
    private final HelloAuthorityUtils authorityUtils;

    ...
    ...

    public Member createMember(Member member) {
        verifyExistsEmail(member.getEmail());
        String encryptedPassword = passwordEncoder.encode(member.getPassword());
        member.setPassword(encryptedPassword);

        //  Role을 DB에 저장
        List<String> roles = authorityUtils.createRoles(member.getEmail());
        member.setRoles(roles);

        Member savedMember = memberRepository.save(member);

        return savedMember;
    }

    ...
    ...
}
```

- `authorityUtils.createRoles(member.getEmail());` : 회원의 권한 정보를 생성한 뒤 member 객체에 넘겨준다.

```java
@Component
public class HelloAuthorityUtils {
    @Value("${mail.address.admin}")
    private String adminMailAddress;

    ...
    ...

    private final List<String> ADMIN_ROLES_STRING = List.of("ADMIN", "USER");
    private final List<String> USER_ROLES_STRING = List.of("USER");

    ...
    ...

    // DB 저장 용. createRoles 메서드 추가
    public List<String> createRoles(String email) {
        if (email.equals(adminMailAddress)) {
            return ADMIN_ROLES_STRING;
        }
        return USER_ROLES_STRING;
    }
}
```

### 로그인 인증 시, User의 권한 정보를 데이터베이스에서 조회하는 작업

```java
@Component
public class HelloUserDetailsService implements UserDetailsService {
    private final MemberRepository memberRepository;
    private final HelloAuthorityUtils authorityUtils;

    public HelloUserDetailsServiceV3(MemberRepository memberRepository, HelloAuthorityUtils authorityUtils) {
        this.memberRepository = memberRepository;
        this.authorityUtils = authorityUtils;
    }

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Member> optionalMember = memberRepository.findByEmail(username);
        Member findMember = optionalMember.orElseThrow(() -> new BusinessLogicException(ExceptionCode.MEMBER_NOT_FOUND));

        return new HelloUserDetails(findMember);
    }

    private final class HelloUserDetails extends Member implements UserDetails {
        HelloUserDetails(Member member) {
            setMemberId(member.getMemberId());
            setName(member.getName());
            setEmail(member.getEmail());
            setPassword(member.getPassword());
						// Member의 데이터베이스에서 조회한 roles를 전달
            setRoles(member.getRoles()); 
        }

        @Override
        public Collection<? extends GrantedAuthority> getAuthorities() {
            // DB에 저장된 Role 정보로 User 권한 목록 생성
            return authorityUtils.createAuthorities(this.getRoles());
        }

        ...
        ...
    }

}
```

```java
@Component
public class HelloAuthorityUtils {
    @Value("${mail.address.admin}")
    private String adminMailAddress;

    private final List<GrantedAuthority> ADMIN_ROLES = AuthorityUtils.createAuthorityList("ROLE_ADMIN", "ROLE_USER");
    private final List<GrantedAuthority> USER_ROLES = AuthorityUtils.createAuthorityList("ROLE_USER");
    private final List<String> ADMIN_ROLES_STRING = List.of("ADMIN", "USER");
    private final List<String> USER_ROLES_STRING = List.of("USER");

    // 메모리 상의 Role을 기반으로 권한 정보 생성.
    public List<GrantedAuthority> createAuthorities(String email) {
        if (email.equals(adminMailAddress)) {
            return ADMIN_ROLES;
        }
        return USER_ROLES;
    }

    // DB에 저장된 Role을 기반으로 권한 정보 생성
    public List<GrantedAuthority> createAuthorities(List<String> roles) {
       List<GrantedAuthority> authorities = roles.stream()
               .map(role -> new SimpleGrantedAuthority("ROLE_" + role))
               .collect(Collectors.toList());
       return authorities;
    }

    ...
    ...
}
```

- `new SimpleGrantedAuthority("ROLE_" + role)` : `SimpleGrantedAuthority` 객체를 생성할 때 생성자 파라미터로 넘겨주는 값이 “ `USER`" 또는 “`ADMIN`"으로 넘겨주면 안되고 “`ROLE_USER`" 또는 “`ROLE_ADMIN`" 형태로 넘겨주어야 한다.

## **핵심 포인트**

- Spring Security에서 지원하는 InMemory User는 말 그대로 메모리에 등록되어 사용되는 User이므로 애플리케이션 실행이 종료되면InMember User 역시 메모리에서 사라진다.
- InMemory User를 사용하는 방식은 **테스트 환경**이나 **데모 환경**에서 사용할 수 있는 방법이다.
- Spring Security는 사용자의 **크리덴셜(Credential, 자격증명을 위한 구체적인 수단)**을 암호화 하기 위한 PasswordEncoder를 제공하며, PasswordEncoder는 다양한 암호화 방식을 제공하며, Spring Security에서 지원하는 PasswordEncoder의 디폴트 암호화 알고리즘은 bcrypt이다.
- 패스워드 같은 **민감한(sensitive) 정보는 반드시 암호화 되어 저장되어야 합니다.**
패스워드는 복호화 할 이유가 없기 때문에 **단방향 암호화** 방식으로 암호화 되어야 한다.
- Spring Security에서 `SimpleGrantedAuthority` 를 사용해 Role 베이스 형태의 권한을 지정할 때 `‘Role_’ + 권한명` 형태로 지정해 주어야 한다.
- Spring Security에서는 Spring Security에서 관리하는 User 정보를 `UserDetails`로 관리한다.
- `UserDetails`는 UserDetailsService에 의해 로드(load)되는 핵심 User 정보를 표현하는 인터페이스입니다.
- `UserDetailsService`는 User 정보를 로드(load)하는 핵심 인터페이스이다.
- 일반적으로 Spring Security에서는 인증을 시도하는 주체를 `User(비슷한 의미로 Principal도 있음)`라고 부른다. Principal은 User의 더 구체적인 정보를 의미하며, 일반적으로 Username을 의미한다.
- Custom UserDetailsService를 사용해 로그인 인증을 처리하는 방식은 **Spring Security가 내부적으로 인증을 대신 처리해주는 방식이다.**
- `AuthenticationProvider`는 Spring Security에서 클라이언트로부터 전달받은 인증 정보를 바탕으로 인증된 사용자인지를 처리하는 Spring Security의 컴포넌트이다.