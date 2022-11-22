* [Filter와 FilterChain 구현](#filter와-filterchain-구현)
    + [Filter](#filter)
    + [FilterChain](#filter-chain)
    + [Filter와 FilterChain의 특성](#filter와-filter-chain의-특성)
    + [Filter 구현 클래스 기본 구조](#filter-구현-클래스-기본-구조)
    + [Filter 실습](#filter-실습)
    + [정리](#정리)


# Filter와 FilterChain 구현

## Filter

> 서블릿 필터(Servlet Filter)는 서블릿 기반 애플리케이션의 엔드포인트에 요청이 도달하기 전에 중간에서 요청을 가로챈 후 어떤 처리를 할 수 있도록 해주는 Java의 컴포넌트이다.
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c805e5f4-3721-4a3c-b924-0141b192258a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221122T135110Z&X-Amz-Expires=86400&X-Amz-Signature=ad178fae1fd84d8ced56173f7169cccbef439394fb8c795f29a53d140ea7e45b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 클라이언트가 서버 측 애플리케이션으로 요청을 전송하면 제일 먼저 **Servlet Filter**를 거치게 된다.
- **Filter에서의 처리가 모두 완료되면** DispatcherServlet에서 클라이언트의 요청을 핸들러에 매핑하기 위한 다음 작업을 진행한다.

## Filter Chain

> Filter Chain은 여러개의 Filter의 체인을 형성하고 있는 Filter의 묶음을 의미한다.
> 

## Filter와 Filter Chain의 특성

- Servlet Filter Chain은 요청 URI path를 기반으로 HttpServletRequest를 처리한다. 클라이언트가 서버 측 애플리케이션에 요청을 전송하면 서블릿 컨테이너는 요청 URI의 경로를 기반으로 어떤 Filter와 어떤 Servlet을 매핑할지 결정한다.
- Filter는 Filter Chain 안에서 순서를 지정할 수 있으며 지정한 순서에 따라서 동작하게 할 수 있다.
- Filter Chain에서 Filter의 순서는 매우 중요하며 Spring Boot에서 여러 개의 Filter를 등록하고 순서를 지정하기 위해서는 두 가지 방법을 적용할 수 있다.
    - Spring Bean으로 등록되는 Filter에 `@Order` 애너테이션을 추가하거나 `Ordered` 인터페이스를 구현해서 Filter의 순서를 지정할 수 있다.
    - `FilterRegistrationBean` 을 이용해 Filter의 순서를 명시적으로 지정할 수 있다.

## Filter 구현 클래스 기본 구조

```java
public class FirstFilter implements Filter {
     // (1) 초기화 작업
     public void init(FilterConfig filterConfig) throws ServletException {
        
     }
     
     // (2)
     public void doFilter(ServletRequest request,
                          ServletResponse response,
                          FilterChain chain)
                          throws IOException, ServletException {
        // (2-1) 이 곳에서 request(ServletRequest)를 이용해 다음 Filter로 넘어가기 전처리 작업을 수행한다.

        // (2-2)
        chain.doFilter(request, response);

        // (2-3) 이 곳에서 response(ServletResponse)를 이용해 response에 대한 후처리 작업을 할 수 있다.
     }
     
     // (3)
     public void destroy() {
        // (5) Filter가 사용한 자원을 반납하는 처리
     }
  }
```

- (1) `init()` : 생성한 Filter에 대한 초기화 작업을 진행할 수 있다.
- (2) `doFilter()` : Filter가 처리하는 실질적인 로직을 구현한다.
- (3) `destroy()` : Filter가 컨테이너에서 종료될 때 호출되는데 주로 Filter가 사용한 자원을 반납하는 처리 등의 로직을 작성하고자 할 때 사용된다.

## Filter 실습

```java
import javax.servlet.*;
import java.io.IOException;

public class FirstFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
        System.out.println("FirstFilter 생성됨");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("========First 필터 시작========");
        chain.doFilter(request, response);
        System.out.println("========First 필터 종료========");
    }

    @Override
    public void destroy() {
        System.out.println("FirstFilter Destory");
        Filter.super.destroy();
    }
}
```

```java
import book.study.security.FirstFilter;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FilterConfiguration {

    @Bean
    public FilterRegistrationBean<FirstFilter> firstFilterRegister()  {
        FilterRegistrationBean<FirstFilter> registrationBean = new FilterRegistrationBean<>(new FirstFilter());
        return registrationBean;
    }
}
```

### 애플리케이션 실행

```java
FirstFilter 생성됨
```

### Controller 핸들러 메서드로 요청

- doFilter → controller 동작 → destroy

```java
========First 필터 시작========
치카치카
========First 필터 종료========
```

### 두 번째 Filter 구현

```java
import javax.servlet.*;
import java.io.IOException;

public class SecondFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
        System.out.println("SecondFilter가 생성되었습니다.");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("==========Second 필터 시작==========");
        chain.doFilter(request, response);
        System.out.println("==========Second 필터 종료==========");
    }

    @Override
    public void destroy() {
        System.out.println("SecondFilter가 사라집니다.");
        Filter.super.destroy();
    }
}
```

```java
import book.study.security.FirstFilter;
import book.study.security.SecondFilter;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Config {

    @Bean
    public FilterRegistrationBean<FirstFilter> firstFilterRegister()  {
        FilterRegistrationBean<FirstFilter> registrationBean = new FilterRegistrationBean<>(new FirstFilter());
        registrationBean.setOrder(1); // (1)
        return registrationBean;
    }

    @Bean
    public FilterRegistrationBean<SecondFilter> secondFilterRegister()  {
        FilterRegistrationBean<SecondFilter> registrationBean = new FilterRegistrationBean<>(new SecondFilter());
        registrationBean.setOrder(2); // (2)
        return registrationBean;
    }

}
```

- `registrationBean.setOrder()` : Filter의 순서를 지정한다. 파라미터로 지정한 숫자가 적을수록 먼저 실행된다.

```java
========First 필터 시작========
==========Second 필터 시작==========
치카치카
==========Second 필터 종료==========
========First 필터 종료========
```

- Filter는 나머지 Filter와 Servlet에 영향을 주기 때문에 **Filter의 실행 순서는 중요하다.**

## 정리

- Spring Boot에서는 `FilterRegistrationBean` 을 이용해 Filter를 등록할 수 있다.
- Spring Boot에서 등록하는 Filter는 다음과 같은 방법으로 실행 순서를 지정할 수 있다.
    - Spring Bean으로 등록되는 Filter에 `@Order` 애너테이션을 추가하거나 `Orderd` 인터페이스로 Filter의 순서를 지정할 수 있다.
    - `FilterRegistrationBean` 의 SetOrder() 메서드를 이용해 Filter의 순서를 지정할 수 있다.