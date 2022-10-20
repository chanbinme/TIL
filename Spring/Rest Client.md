* [목차](#목차)
* [Rest Client](#rest-client)
    + [Rest Client란?](#rest-client란)
    + [RestTemplate](#resttemplate)
    + [RestTemplate으로 World Time API 사용해보기](#resttemplate으로-world-time-api-사용해보기)
        + [RestTemplate 객체 생성](#resttemplate-객체-생성)
        + [URI 생성](#uri-생성)
        + [요청 전송](#요청-전송)
        + [정리](#정리)
        + [적용할만한 오픈 API](#적용할만한-오픈-api)

# Rest Client

## Rest Client란?

- Rest API 서버에 HTTP 요청을 보낼 수 있는 클라이언트 툴 또는 라이브러리를 의미한다.
- 대표적으로 Postman이 있다.

## RestTemplate

- Java에서 사용할 수 있는 HTTP Client 라이브러리로는 java.net.HttpURLConnection, Apache HttpComponents, OkHttp 3, Netty등이 있다.
- Spring에서는 위에서 언급한 HTTP Client 라이브러리 중 하나를 이용해 **원격지에 있는 다른 Backend 서버에 HTTP 요청을 보낼 수 있는 RestTemplate**이라는 Rest Client API를 제공한다.

## RestTemplate으로 **[World Time API](http://worldtimeapi.org) 사용해보기**

### RestTemplate 객체 생성

```java
public class RestClientExample01 {
    public static void main(String[] args) {
        // 객체 생성
        RestTemplate restTemplate = 
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());
    }
}
```

- RestTemplate의 객체를 생성하기 위해서는 `RestTemplate` 의 생성자 파라미터로 HTTP Client 라이브러리의 구현 객체를 전달해야 한다.
- 여기서는 Apache HttpComponents를 사용하기 위해`HttpComponentsClientHttpRequestFactory` 를 넣었다.
- Apache HttpComponents를 사용하기 위해서는 build.grad의 dependencies 항목에 의존 라이브러리를 추가해야한다.

```java
dependencies {
    ...
    implementation 'org.apache.httpcomponents:httpclient'
}
```

### URI 생성

```java
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.util.UriComponents;
import org.springframework.web.util.UriComponentsBuilder;

import java.net.URI;

public class RestClientExample01 {
    public static void main(String[] args) {

        RestTemplate restTemplate =
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());

        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();
    }
}
```

- `UriComponentBuilder` 클래스로 `UriComponents` 객체를 생성 → `UriComponents` 객체를 이용해서 HTTP Request를 요청할 엔드포인트의 URI 생성
- `UriComponentBuilder` 클래스에서 제공하는 API 메서드의 기능
    - `newInstance()` : `UriComponentBuilder` 객체를 생성한다.
    - `scheme()` : URI의 scheme를 설정한다.
    - `host()` : 호스트 정보를 입력한다.
    - `port()` : 디폴트 값은 80이므로 80포트를 사용하는 호스트는 생략 가능하다.
    - `path()` :
        - URI의 경로(path)를 입력한다.
        - 위에서 URI의 path에 {continents}, {city}의 두 개의 템플릿 변수를 사용하고 있다.
        - 두 개의 템플릿 변수는 `uriComponents.expand("Asia", "Seoul").toUri();`에서 `expand()`메서드 파라미터의 문자열로 채워진다.
        - 빌드 타임에 {continents}는 ‘Asia’, {city}는 ‘Seoul’로 변환한다.
    - `encode()` : URI에 사용된 템플릿 변수들을 인코딩 해준다.
    - `build()` : `UriComponents` 객체를 생성한다.
- `UriComponents` 에 사용된 API 메서드의 기능
    - `expend()` : 파라미터로 입력한 값을 URI 템플릿 변수의 값으로 대체한다.
    - `toUri()` : `Uri` 객체를 생성한다.

### 요청 전송

`getForObject()` **를 이용해서 문자열 응답 데이터 전달 받기**

```java
public class RestClientExample01 {
    public static void main(String[] args) {
				// 객체 생성
        RestTemplate restTemplate =
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());

				// URI 생성
        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();

				// Request 전송
        String result = restTemplate.getForObject(uri, String.class);

        System.out.println(result);
    }
}
```

- 기능 설명
    - `getForObject()` 메서드는 HTTP Get 요청을 통해 서버의 리소스를 조회한다.
- 파라미터 설명
    - `URI uri` : Request를 전송할 엔드포인트의 URI 객체를 지정해준다.
    - `Class<T> responseType` : 응답으로 전달 받을 클래스의 타입/을 지정해준다.  여기서는 `String.class` 로 지정

```java
// 코드 실행 결과

abbreviation: KST
client_ip: 175.211.255.15
datetime: 2022-10-21T00:30:29.854331+09:00
day_of_week: 5
day_of_year: 294
dst: false
dst_from: 
dst_offset: 0
dst_until: 
raw_offset: 32400
timezone: Asia/Seoul
unixtime: 1666279829
utc_datetime: 2022-10-20T15:30:29.854331+00:00
utc_offset: +09:00
week_number: 42
```

`getForObject()` **를 이용한 커스텀 클래스 타입으로 원하는 정보만 응답으로 받기**

```java
public class RestClientExample02 {
    public static void main(String[] args) {
        RestTemplate restTemplate =
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());

        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();

        // 원하는 데이터만 전달 받고 싶으면 클래스를 별도로 생성해서 원하는 데이터를 전달 받을 수 있다.
        WorldTime worldTime = restTemplate.getForObject(uri, WorldTime.class);

        System.out.println("# datatime: " + worldTime.getDatetime());
        System.out.println("# timezone: " + worldTime.getTimezone());
        System.out.println("# day_of_week " + worldTime.getDay_of_week());
    }
```

- 원하는 데이터만 전달 받고 싶으면 문자열을 조작해서 정보를 얻어야 하는데 그러기 위한 로직이 너무 복잡하다.
- 이 경우에는 클래스를 별도로 생성해서 원하는 데이터만 손쉽게 전달 받을 수 있다.

```java
public class WorldTime {
    private String datetime;
    private String timezone;
    private int day_of_week;

    public String getDatetime() {
        return datetime;
    }

    public String getTimezone() {
        return timezone;
    }

    public int getDay_of_week() {
        return day_of_week;
    }
}
```

```java
// 결과
# datatime: 2022-10-21T00:35:31.075250+09:00
# timezone: Asia/Seoul
# day_of_week 5
```

`getForEntity()`**를 사용한 Response Body(바디, 컨텐츠) + Header(헤더) 정보 전달 받기**

```java
// getForEntity()를 이용한 Respons Body(바디, 컨텐츠) + Header(헤더) 정보 전달 받기
public class RestClientExample03 {
    public static void main(String[] args) {
        RestTemplate restTemplate =
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());

        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();

        ResponseEntity<WorldTime> response =
                restTemplate.getForEntity(uri, WorldTime.class);

        System.out.println("# datetime: " + response.getBody().getDatetime());
        System.out.println("# timezone: " + response.getBody().getTimezone());
        System.out.println("# day_of_week " + response.getBody().getDay_of_week());
        System.out.println("# HTTP Status Code: " + response.getStatusCode());
        System.out.println("# HTTP Status Value: " + response.getStatusCodeValue());
        System.out.println("# Content Type: " + response.getHeaders().getContentType());

    }
}
```

- `getForEntity()` 메서드는 헤더 정보와 바디 정보를 모두 전달 받을 수 있다.
- 응답 데이터는 ResponseEntity 클래스로 래핑되어서 전달되며 `getBody()` , `getHeaders()` 메서드 등을 이용해서 바디와 헤더 정보를 얻을 수 있다.
- 모든 헤더 정보를 보고 싶다면 `getHeader().entrySet()` 메서드 사용

**`exchange()` 를 사용한 응답 데이터 받기**

```java
// exchange()를 사용한 응답 데이터 받기
public class RestClientExample04 {
    public static void main(String[] args) {
        RestTemplate restTemplate =
                new RestTemplate(new HttpComponentsClientHttpRequestFactory());

        UriComponents uriComponents =
                UriComponentsBuilder
                        .newInstance()
                        .scheme("http")
                        .host("worldtimeapi.org")
                        .port(80)
                        .path("/api/timezone/{continents}/{city}")
                        .encode()
                        .build();
        URI uri = uriComponents.expand("Asia", "Seoul").toUri();

        /**
         * getForObject(), getForEntity()등과 달리 exchange() 메서드는
         * HTTP Method, RequestEntity, ResponseEntity를 직접 지정해서 HTTP Request를
         * 전송할 수 있는 가장 일반적인 방식이다.
         *
         * URI uri : Request를 전송할 엔드포인트의 URI 객체를 지정해준다.
         * HttpMethod method : HTTP Method 타입을 지정해준다.
         * HttpEntity<?> requestEntity : HttpEntity 객체를 지정해준다. HttpEntity 객체를 통해 헤더 및 바디, 파라미터 등을 설정해줄 수 있다.
         * Class<T> responseType : 응답으로 전달 받을 클래스의 타입을 지정해준다.
         */
        ResponseEntity<WorldTime> response =
                restTemplate.exchange(uri,
                        HttpMethod.GET,
                        null,
                        WorldTime.class);

        System.out.println("# datetime: " + response.getBody().getDatetime());
        System.out.println("# timezone: " + response.getBody().getTimezone());
        System.out.println("# day_of_week " + response.getBody().getDay_of_week());
        System.out.println("# HTTP Status Code: " + response.getStatusCode());
        System.out.println("# HTTP Status Value: " + response.getStatusCodeValue());
    }
}
```

## 정리

- **Rest Client란 Rest API 서버에 HTTP 요청을 보낼 수 있는 클라이언트 툴 또는 라이브러리**를 말한다.
- **RestTemplate**은 원격지에 있는 다른 백엔드 서버에 HTTP요청을 전송할 수 있는 **Rest Client API**이다.
- RestTemplate 사용 단계
    1. RestTemplate 객체를 생성한다.
    2. HTTP 요청을 전송할 엔드포인트의 URI 객체를 생성한다.
    3. `getForObject()`, `getForEntity()`, `exchange()` 등을 이용해서 HTTP 요청을 전송한다.
- RestTemplate을 사용할 수 있는 기능 예
    - 결제 서비스
    - 카카오톡 등의 메시징 서비스
    - Google Map 등의 지도 서비스
    - 공공 데이터 포털, 카카오, 네이버 등에서 제공하는 Open API
    - 기타 원격지 API 서버와의 통신

## 적용할만한 오픈 API

- 공공 데이터 포털: [https://www.data.go.kr/dataset/3043385/openapi.do](https://www.data.go.kr/dataset/3043385/openapi.do)
- 카카오 REST API: [https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api)
- 네이버 API: [https://developers.naver.com/products/intro/plan/plan.md](https://developers.naver.com/products/intro/plan/plan.md)
- 구글 API 서비스: [https://console.cloud.google.com](https://console.cloud.google.com/)
- 공공 인공지능 API 서비스: [https://aiopen.etri.re.kr/](https://aiopen.etri.re.kr/)