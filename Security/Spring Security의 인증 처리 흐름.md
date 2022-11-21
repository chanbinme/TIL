# Spring Security의 인증 처리 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2f60794c-60a1-497f-9159-f8c70e1e2260/KakaoTalk_Photo_2022-11-21-17-41-18.jpeg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221121T131403Z&X-Amz-Expires=86400&X-Amz-Signature=41b455ee1a2036c1148709bd43129965f5371097d8f6236648cf72b2e95f1cdb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22KakaoTalk_Photo_2022-11-21-17-41-18.jpeg%22&x-id=GetObject)

1. 사용자가 Username(로그인 ID)과 Password를 포함한 request를 Spring Security가 적용된 애플리케이션에 전송한다.
    
    사용자의 로그인 요청이 Spring Security의 Filter Chain까지 들어오면 여러 Filter들 중에서 `UsernamePasswordAuthenticationFilter` 가 해당 요청을 전달 받는다. 
    
2. 요청을 전달 받은 `UsernamePasswordAuthenticationFilter` 는 Username과 Password를 이용해 `UsernamePasswordAuthenticationToken` 을 생성한다.
    
    `UsernamePasswordAuthenticationToken` 은 `Authentication` 인터페이스를 구현한 구현 클래스이다.
    
3. 아직 인증되지 앟은 `Authentication` 을 가지고 있는 `UsernamePasswordAuthenticationFilter` 는 `Authentication` 을 `AuthenticationManager` 에게 전달한다.
    
    `AuthenticationManager` 는 인증 처리를 총괄하는 매니저 역할을 하는 인터페이스이고, `AuthenticationManager` 를 구현한 구현 클래스가 바로 `ProviderManager` 이다. 
    
    즉, `ProviderManager` 가 인증이라는 작업을 총괄하는 실질적인 매니저라고 볼 수 있다.
    
4. `ProviderMangaer` 로부터 `Authentication` 을 전달 받은 `AuthenticationProvider` 는 `UserDetailsService` 을 이용해 `UserDetails` 를 조회한다.
    
    `UserDetails` 는 데이터베이스 등의 저장소에 저장되 사용자의 Username과 사용자의 자격을 증명해주는 `Credential` 인 Password, 그리고 사용자의 권한 정보를 포함하고 있는 컴포넌트이다. 
    
    그리고 이 `UserDetails` 를 제공하는 컴포넌트가 바로 `UserDetailsService` 이다.
    
5. `UserDetailsService` 는 데이터베이스 등의 저장소에서 사용자의 `Credential` 을 포함한 사용자의 정보를 조회한다. 
6. 데이터베이스 등의 저장소에서 조회한 사용자의 `Credential` 을 포함한 사용자의 정보를 기반으로 `UserDetails` 를 생성한 후, 생성된 `UserDetails` 를 다시 `AuthenticationProvider` 에게 전달한다. 
7. `UserDetails` 를 전달 받은 `AuthenticationProvider` 는 PasswordEncoder를 이용해 `UserDetails` 에 포함된 암호화 된 Password와 인증을 위한 `Authentication` 안에 포함된 Password가 일치하는지 검증한다. 
    - 검증에 성공하면 `UserDetails` 를 이용해 인증된 `Authentication` 을 생성한다.
    - 검증에 성공하지 못하면 Exception을 발생시키고 인증 처리를 중단한다.
8. `AuthenticationProvider` 는 인증된 `Authentication` 을 `ProviderManager` 에게 전달한다.
    
    `Authentication` 은 인증을 위해 필요한 사용자의 로그인 정보를 가지고 있지만, 이 단계에서 `ProviderManager` 에게 전달한 `Authentication` 은 인증에 성공한 사용자의 정보(Principal, Credentail, GrantedAuthorities)를 가지고 있다. 
    
9. 이제 `ProviderManager` 는 인증된 `Authentication` 을 다시 `UsernamePasswordAuthenticationFilter` 에게 전달한다.
10. 인증된 `Authentication` 을 전달 받은 `UsernamePasswordAuthenticationFilter` 는 마지막으로 `SecurityContextHolder` 를 이용해 `SecurityContext` 에 인증된 `Authentication` 을 저장한다.

## 핵심 포인트

- `UsernamePasswordAuthenticationFilter` : 사용자의 로그인 요청을 처리하는 Spring Security Filter
- `UsernamePasswordAuthenticationToken` : `Authentication` 인터페이스를 구현한 구현 클래스
- `AuthenticationManager` : 인증 처리를 총괄하는 매니저 역할을 하는 인터페이스. `AuthenticationManager` 를 구현한 구현 클래스가 `ProviderManager` 이다.
- `UserDetailse` : 데이터베이스 등의 저장소에 저장된 사용자의 username과 사용자의 자격을 증명해주는 `Credentail` 인 Password, 그리고 사용자의 권한 정보를 포함하고 있는 컴포넌트이다.
- `UserDetailsService` : `UserDetails` 를 제공하는 컴포넌트. 데이터베이스 등의 저장소에서 사용자의 `Credentail` 을 포함한 사용자의 정보를 조회하여 `AuthenticationProvider` 에게 제공한다.
- `UsernamePasswordAuthenticationFilter` 가 생성한 Authentication은 인증을 위해 필요한 사용자의 정보를 갖고있다.
- `AuthentcationProvider` 가 생성한 `Authentication` 은 인증에 성공한 사용자의 정보를 가지고 있다.
- 인증된 `Authentication`을 전달 받은 `UsernamePasswordAuthenticationFilter` 는 `SecurityContextHolder` 를 이용해 `SecurityContext` 에 인증된 `Authentication` 을 저장한다. `SecurityContext` 는 다시 HttpSession에 저장되어 사용자의 인증 상태를 유지한다.