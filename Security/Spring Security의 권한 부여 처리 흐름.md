# Spring Security의 권한 부여 처리 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/69f053b8-66b9-4613-b15a-b47c8725de35/Untitled.jpeg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221121T131620Z&X-Amz-Expires=86400&X-Amz-Signature=c5e346f859e0259ac0d30818aa00fb4cb64e87d3529644c28d4afd0f7b586509&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.jpeg%22&x-id=GetObject)

- Spring Security Filter Chain에서 URL을 통해 사용자의 액세스를 제한하는 권한 부여 Filter는 바로 `AuthorizationFilter` 이다.
- `AuthorizationFilter` 는 먼저 (1)과 같이 SecurityContextHolder로 부터 Authentication을 획득한다.
- (2)와 같이 SecurityContextHolder로부터 획득한 Authentication과 HttpServletRequest를 `AuthorizationManager`에게 전달한다.
- `AuthorizationManager` 는 권한 부여 처리를 총괄하는 매니저 역할을 하는 인터페이스이고, `RequestMatcherDelegatingAuthorizationManager` 는 `AuthorizationManager` 를 구현하는 구현체 중 하나이다.
    - `RequestMatcherDelegatingAuthorizationManager` 는 `RequestMatcher` 평가식을 기반으로 해당 평가식에 매치되는 `AuthorizationManager` 구현 클래스에게 위임만 한다.
- (3)`RequestMatcherDelegatingAuthorizationManager` 내부에서 매치되는 `AuthorizationManager` 구현 클래스가 있다면 해당 `AuthorizationManager` 구현 클래스가 사용자의 권한을 체크한다.
- 적절한 권한이라면 (4)와 같이 다음 요청 프로세스를 계속 이어간다.
- 적절한 권한이 아니라면 (5)와 같이 `AccessDeniedException` 이 throw되고 ExceptionTranslationFilter가 `AccessDeinedException` 을 처리하게 된다.