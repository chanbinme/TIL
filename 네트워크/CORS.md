# 목차
* [목차](#목차)
* [* [REST API](#cors)
    + [CORS(Cross-Origin Resource Sharing)](#corscross-origin-resource-sharing)
	+ [CORS 정책 동작 방식](#cors-정책-동작-방식)
	+ [@CrossOrigin 애너테이션을 이용해 CORS 정책 설정 방법](#crossorigin-애너테이션을-이용해-cors-정책-설정-방법)

# CORS

## ****CORS(Cross-Origin Resource Sharing)****

- 애플리케이션 간에 출처(Origin)가 다른 경우, 스크립트 기반의 HTTP 통신을 통한 리소스 접근이 제한되는 정책(SOP; Same Origin Policy)이 있다.
- CORS는 이러한 접근 제한의 예외 조항으로써, 사전 설정을 통해 리소스에 선택적으로 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 정책이다.
- 즉, 브라우저는 SOP에 의해 기본적으로 다른 출처의 리소스 공유를 막지만, CORS를 사용하면 접근 권한을 얻을 수 있게 된다는 것
- CORS 관련 에러가 발생한다면 아래와 같은 콘솔 문구를 볼 수 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0b26301b-0d3f-48b5-b885-cace0b48eb78/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221213%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221213T071530Z&X-Amz-Expires=86400&X-Amz-Signature=4862f15cf80e4e18e14c308669c45027c1b767381ddb415ee3aecafe6cb94220&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

> 다른 출처의 리소스를 가져오려고 했지만 SOP 때문에 접근이 불가능하다. CORS 설정을 통해 서버의 응답 헤더에 ‘Access-Control-Allow-Origin’을 작성하면 접근 권한을 얻을 수 있다.
> 
- 즉, 이 에러는 CORS가 아닌 SOP때문이다.

## CORS 정책 동작 방식

- CORS의 동작 방식에는 크게 세 가지가 있다.

### 프리플라이트 요청(Preflight Request)

- 실제 요청을 보내기 전, OPTIONS 메서드로 사전 요청을 보내 해당 출처 리소스에 접근 권한이 있는지 확인하는 것을 의미한다.
- 브라우저는 서버에 실제 요청을 보내기 전에 프리플라이트 요청을 보내고, 응답 헤더의 `Access-Control-Allow-Origin` 으로 요청을 보내 출처가 돌아오면 실제 요청을 보내게 된다.
- 만약에 요청을 보낸 출처가 접근 권한이 없다면 브라우저에서 CORS 에러를 띄우게 되고, 실제 요청은 전달되지 않는다.

### 단순 요청(Simple Request)

- 단순 요청은 특정 조건이 만족되면 프리플라이트 요청을 생략하고 요청을 보내는 것을 말한다.

### 인증정보를 포함한 요청(Credentialed Request)

- 요청 헤더에 인증 정보를 담아 보내는 요청이다.
- 출처가 다를 경우에는 별도의 설정을 하지 않으면 쿠키를 보낼 수 없다. 민감한 정보이기 때문이다.
- 이 경우에 프론트, 서버 양측 모두 CORS 설정이 필요하다.

## @CrossOrigin 애너테이션을 이용해 CORS 정책 설정 방법

- `@CrossOrigin` 애너테이션을 이용해 컨트롤러 혹은 메서드에서 CORS 정책을 설정
- 애너테이션이 붙은 컨트롤러(혹은 메서드)에서만 적용되며 따라서 원하는 요청에 따른 응답에만 CORS 설정을 할 수 있다.

```java
@CrossOrigin // Controller에 애너테이션을 이용해 설정합니다.
@RestController
public class HelloController {

	...

}
```

```java
@RestController
public class HelloController {

	@CrossOrigin // 해당하는 매서드에 애너테이션을 이용해 설정합니다.
	@GetMapping
	public ResponseEntity<> getHello () {
		...
	}

}
```

- 애너테이션을 이용해 CORS 설정을 하는 경우, 옵션을 이용해 세부적인 설정을 추가할 수 있다.

```java
@CrossOrigin(origins = "https://codestates.com")
@RestController
public class HelloController {
	...
}
```