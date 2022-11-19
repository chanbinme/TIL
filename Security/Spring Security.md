* [Spring Security](#spring-security)
    + [Spring Security로 할 수 있는 보안 강화 기능](#spring-security로-할-수-있는-보안-강화-기능)
    + [Spring Security에서 사용하는 용어](#spring-security에서-사용하는-용어)
        + [Principal(주체)](#principal주체)
        + [Authentication(인증)](#authentication인증)
        + [Authorization(인가 또는 권한)](#authorization인가-또는-권한-부여)
        + [Access Control(접근 제어)](#access-control접근-제어)
    + [Spring Security를 사용해야 하는 이유](#spring-security를-사용해야-하는-이유)

# Spring Security

> Spring Security는 Spring MVC 기반 애플리케이션의 인증(Authentication)과 인가(Authorizationor 권한 부여) 기능을 지원하는 보안 프레임워크이다.
> 
- Spring MVC 기반 애플리케이션에 보안을 적용하기위한 사실상의 표준이다.

## Spring Security로 할 수 있는 보안 강화 기능

- 다양한 유형(폼 로그인 인증, 토큰 기반 인증, OAuth 2 기반 인증, LDAP 인증)의 사용자 인증 기능 적용
- 애플리케이션 사용자의 역할(Role)에 따른 권한 레벨 적용
- 애플리케이션에서 제공하는 리소스에 대한 접근 제어
- 민감한 정보에 대한 데이터 암호화
- SSL 적용
- 일반적으로 알려진 웹 보안 공격 차단

이 외에도 SSO, 클라이언트 인증 기반 인증, 메서드 보안, 접근 제어 목록(Access Control List) 같은 보안을 위한 대부분의 기능을 지원한다.

## Spring Security에서 사용하는 용어

### Principal(주체)

- Spring Security에서 사용되는 `Principal` 은 애플리케이션에서 **작업을 수행할 수 있는 사용자, 디바이스 또는 시스템** 등이 될 수 있다.
- 일반적으로 인증 프로세스가 성공적으로 수행된 사용자의 계정 정보를 의미한다.

### Authentication(인증)

- `Authentication` 은 애플리케이션을 사용하는 사용자가 본인이 맞음을 증명하는 절차를 의미한다.
- `Authentication` 을 정상적으로 수행하기 위해서는 사용자를 식별하기 위한 정보가 필요한데 이를 `Credential` (신원 증명 정보)이라고 한다.
- 예를 들어 주민센터 직원이 나의 신원을 확인하기 위해 요청하는 주민등록증이 나의 `Credential` 이 될 수 있다.
- 특정 사이트에서 로그인을 위해 입력하는 패스워드 역시 로그인 아이디를 증명하기 위한 `Credential` 이 된다.

### Authorization(인가 또는 권한 부여)

- `Authorization` 은 Authentication이 정상적으로 수행된 사용자에게 하나 이상의 권한을 부여하여 특정 애플리케이션의 특정 리소스에 접근할 수 있게 허가하는 과정을 의미한다.
- `Authorization` 은 반드시 Authentication 과정 이후 수행되어야 하며 권한은 일반적으로 역할(Role) 형태로 부여된다.

### Access Control(접근 제어)

- `Access Control` 은 사용자가 애플리케이션의 리소스에 접근하는 행위를 제어하는 것을 의미한다.

## Spring Security를 사용해야 하는 이유

- 보안은 주제는 소프트웨어 세계에서 가장 어려운 주제 중 하나이다. 그렇기 때문에 직접 보안을 구현하기보다 신뢰할만한 Spring Security를 사용하는 것이 효율적이다.(Spring Security도 어렵다;;)
- Spring 내에서 보안을 강화하기 위한 솔루션으로 Spring Security만한 다른 프레임워크가 존재하지 않는다.
- Spring Security에서 지원하는 기본 옵션을 통해 대부분의 보안 요구 사항을 만족시킬 수 있다.
- Spring Security는 특정 보안 요구 사항을 만족하기 위한 커스터마이징이 용이하고, 유연한 확장이 가능하다.