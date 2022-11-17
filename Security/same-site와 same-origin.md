* [same-site와 same-origin 이해하기](#same-site와-same-origin-이해하기)
    + [origin](#origin)
        + [same-origin과 cross-origin](#same-origin과-cross-origin)
    + [site](#site)
        + [same-site와 cross-site](#same-site와-cross-site)
    + [schemeful same-site](#schemeful-same-site)
    + [request가 same-site, same-origin 또는 cross-site인지 확인하는 방법](#request가-same-site-same-origin-또는-cross-site인지-확인하는-방법)
# same-site와 same-origin 이해하기

## origin

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/491f272d-1afc-495f-859e-c0e3f4190bc4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221117T135449Z&X-Amz-Expires=86400&X-Amz-Signature=438dd6d66eef7926971b9481d95f041d64633be219bd403edacf30585c215f56&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- origin은 프로토콜(HTTP, HTTPS), 호스트 이름 및 포트의 조합이다
- 예를 들어, URL이 `[https://www.localhost.com:443/foo](https://www.localhost.com:443/foo)` 인 경우 origin은 `https://www.localhost.com:443` 이다.

### same-origin과 cross-origin

- 동일한 프로토콜, 도메인 및 포트가 조합된 웹 사이트는 same-origin으로 간주된다.
- 다른 모든 것은 cross-origin으로 간주된다.

| 출처A | 출처B | A와 B가 same-origin인지 cross-origin인지에 대한 설명 |
| --- | --- | --- |
| https://www.example.com:443 | https://www.evil.com :443 | cross-origin : 다른 도메인 |
|  | https://example.com:443 | cross-origin : 다른 하위 도메인 |
|  | https://login.example.com:443 | cross-origin : 다른 하위 도메인 |
|  | http://www.example.com:443 | cross-origin : 다른 프로토콜 |
|  | https://www.example.com:80 | cross-origin : 다른 포트 |
|  | https://www.example.com:443 | same-origin |
|  | https://www.example.com | same-origin : 암시적 포트 번호(443) 일치 |

## site

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f8d892ae-2370-4700-b6d9-749203f8bfe7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221117T135503Z&X-Amz-Expires=86400&X-Amz-Signature=d2b21f4bdf67cc79b6b7eca2010968aae14bdec0a55e95ac2e524500190c8b8b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- `.com` 및 `.org` 와 같은 최상위 도메인(TLD)은 루트 영역 데이터베이스에 나열된다. 위의 예에서 site는 TLD와 그 바로 앞의 도메인 부분의 조합이다.
- 예를 들어 URL이 `[https://www.example.com:443/foo](https://www.example.com:443/foo)` 인 경우 site는 `example.com` 이다.
- `.co.jp` 또는 `.github.io` 와 같은 `.jp` 또는 `.io` 의 TLD를 사용하는 것만으로는 site를 식별할 수 있을 만큼 세분화되지 않았다.
- 전체 사이트 이름은 eTLD+1로 불린다.
- 예를 들어 URL이 `[http://my-project.github.io](http://my-project.github.io)` 인 경우
    - eTLD : `.github.io`
    - eTLD+1(site) : `[my-project.github.io](http://my-project.github.io)`

### same-site와 cross-site

- same-site : 동일한 eTLD+1이 있는 웹사이트
- cross-site : 다른 eTLD+1이 있는 웹사이트

| 출처A | 출처B | A와 B가 same-site인지 cross-site인지에 대한 설명 |
| --- | --- | --- |
| https://www.example.com:443 | https://www.evil.com :443 | cross-site : 다른 도메인 |
|  | https://example.com:443 | same-site : 다른 하위 도메인은 중요하지 않다. |
|  | https://login.example.com:443 | same-site : 다른 하위 도메인은 중요하지 않다. |
|  | http://www.example.com:443 | same-site : 다른 프로토콜은 중요하지 않다. |
|  | https://www.example.com:80 | same-site : 다른 포트는 중요하지 않다. |
|  | https://www.example.com:443 | same-site : 정확히 일치 |
|  | https://www.example.com | same-site : 포트는 중요하지 않다. |

## schemeful same-site

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d7228710-51de-492a-af17-06bd10815f24/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221117T135528Z&X-Amz-Expires=86400&X-Amz-Signature=c7aac475a70708d8673b1e4950063a217c1a1d8e51c17db8dece0832a7bbf0d7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- same-site는 HTTP가 약한 채널로 사용되는 것을 방지하기 위해 URL 프로토콜을 site의 일부로 간주하도록 진화하고 있다.
- 이 경우 `[http://www.example.com](http://www.example.com)` 및 `[https://www.example.com](https://www.example.com)` 이 교차 사이트로 간주된다.

| 출처A | 출처B | A와 B가 schemeful same-site에 대한 설명 |
| --- | --- | --- |
| https://www.example.com:443 | https://www.evil.com :443 | cross-site : 다른 도메인 |
|  | https://login.example.com:443 | schemeful same-site : 다른 하위 도메인은 중요하지 않다. |
|  | http://www.example.com:443 | cross-site : 다른 프로토콜 |
|  | https://www.example.com:80 | schemeful 동일 사이트: 다른 포트는 중요하지 않다. |
|  | https://www.example.com:443 | schemeful same-site : 정확히 일치 |
|  | https://www.example.com | scheme same-site : 포트는 중요하지 않다. |

## request가 same-site, same-origin 또는 cross-site인지 확인하는 방법

- `Sec-Fetch-Site` HTTP 헤더와 함께 요청을 보낸다.
- `Sec-Fetch-Site` 의 값을 검사하여 요청이 `cross-site`, `same-site` 또는 `same-origin` 인지 확인할 수 있다.
- `schemeful-same-site` 는 `Sec-Fetch-Site` 에서 확인되지 않는다.
- 헤더에는 다음 값 중 하나가 있다.
    - `cross-site`
    - `same-site`
    - `same-origin`
    - `none`