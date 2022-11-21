# Spring Security의 권한 부여 컴포넌트

## AuthorizationFilter

> `AuthorizationFilter` 는 URL을 통해 사용자의 액세스를 제한하는 권한 부여 Filter이며, Spring Security 5.5 버전부터 FilterSecurityInterceptor를 대체한다.
> 

```java
public class AuthorizationFilter extends OncePerRequestFilter {

	private final AuthorizationManager<HttpServletRequest> authorizationManager;
  
  ...
  ...
	
  // (1)
	public AuthorizationFilter(AuthorizationManager<HttpServletRequest> authorizationManager) {
		Assert.notNull(authorizationManager, "authorizationManager cannot be null");
		this.authorizationManager = authorizationManager;
	}

	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		AuthorizationDecision decision = this.authorizationManager.check(this::getAuthentication, request); // (2)
		this.eventPublisher.publishAuthorizationEvent(this::getAuthentication, request, decision);
		if (decision != null && !decision.isGranted()) {
			throw new AccessDeniedException("Access Denied");
		}
		filterChain.doFilter(request, response);
	}

  ...
  ...

}
```

- (1) AuthroizationFilter 객체가 생성될 때, AuthorizationManager를 DI 받는다. DI 받은 AuthorizationManager를 통해 권한 부여 처리를 진행한다.
- (2) DI 받은 AuthorizationManager의 check() 메서드를 호출해 적절한 권한 부여 여부를 체크한다.
- AuthorizationManager의 check() 메서드는 AuthorizationManager 구현 클래스에 따라 권한 체크 로직이 다르다.
- URL 기반으로 권한 부여 처리를 하는 AuthorizationManager의 구현 클래스로 `RequestMatcherDelegatingAuthorizationManager` 를 사용한다.

## AuthorizationManager

> `AuthorizationManager` 는 이름 그대로 권한 부여 처리를 총괄하는 매니저 역할을 하는 인터페이스이다.
> 

```java
@FunctionalInterface
public interface AuthorizationManager<T> {
  ...
  ...

	@Nullable
	AuthorizationDecision check(Supplier<Authentication> authentication, T object);

}
```

- AuthorizationManager 인터페이스는 check() 메서드 하나만 정의되어 있으며, Supplier와 제너릭 타입의 객체를 파라미터로 가진다.

## RequestMatcherDelegatingAuthorizationManager

> `RequestMatcherDelegatingAuthorizationManager` 는 AuthorizationManager의 구현 클래스 중 하나이며, 직접 권한 부여 처리를 수행하지 않고, `RequestMatcher` 를 통해 매치되는 `AuthorizationManager` 구현 클래스에게 권한 부여 처리를 위임한다.
> 

```java
public final class RequestMatcherDelegatingAuthorizationManager implements AuthorizationManager<HttpServletRequest> {

  ...
  ...

	@Override
	public AuthorizationDecision check(Supplier<Authentication> authentication, HttpServletRequest request) {
		if (this.logger.isTraceEnabled()) {
			this.logger.trace(LogMessage.format("Authorizing %s", request));
		}

    // (1)
		for (RequestMatcherEntry<AuthorizationManager<RequestAuthorizationContext>> mapping : this.mappings) {

			RequestMatcher matcher = mapping.getRequestMatcher(); // (2)
			MatchResult matchResult = matcher.matcher(request);
			if (matchResult.isMatch()) {   // (3)
				AuthorizationManager<RequestAuthorizationContext> manager = mapping.getEntry();
				if (this.logger.isTraceEnabled()) {
					this.logger.trace(LogMessage.format("Checking authorization on %s using %s", request, manager));
				}
				return manager.check(authentication,
						new RequestAuthorizationContext(request, matchResult.getVariables()));
			}
		}
		this.logger.trace("Abstaining since did not find matching RequestMatcher");
		return null;
	}
}
```

- (1) check() 메서드 내부에서 루프를 돌면서 `RequestMatcherEntry` 정보를 얻은 후 (2) `RequestMatcher` 객체를 얻는다.
- (3) MatcherResult.isMatch()가 true이면 AuthorizationManager 객체를 얻은 뒤, 사용자의 권한을 체크한다.
- 여기서 `RequestMatcher` 는 SecurityConfiguration에서 `.antMatchers("/orders/**").hashRole("ADMIN")` 와 같은 메서드 체인 정보를 기반으로 생성된다.

## 핵심 포인트

- `AuthorizationFilter` 는 URL을 통해 사용자의 액세스를 제한하는 권한 부여 Filter이며, Spring Security 5.5 버전부터 FilterSecurityInterceptor를 대체했다.
- `AuthorizationManager` 는 이름 그대로 권한 부여 처리를 총괄하는 매니저 역할을 하는 인터페이스이다.
- `RequestMatcherDelegatingAuthorizationManager` 는 AuthorizationManager의 구현 클래스 중 하나이며, 직접 권한 부여 처리를 수행하지 않고 `RequestMatcher` 를 통해 매치되는 `AuthorizationManager` 구현 클래스에게 권한 부여 처리를 위임한다.
    - `RequestMatcher` 는 SecurityConfiguration에서 `.antMatchers("/orders/**").hasRole("ADMIN")` 와 같은 메서드 체인 정보를 기반으로 생성된다.