* [목차](#목차)
* [JWT(JSON Web Token)](#jwtjson-web-token)
    + [장점](#장점)
    + [단점](#단점)
    + [JWT의 종류](#jwt의-종류)
    + [JWT 구조](#jwt-구조)
    + [토큰 기반 인증 절차](#토큰-기반-인증-절차)

# JWT(JSON Web Token)

> JWT(JSON Web Token)는 JSON 포맷의 토큰 정보를 인코딩 후, 인코딩 된 토큰 정보를 Secret Key로 서명(Sign)한 메시지를 Web Token으로써 인증 과정에 사용한다.
> 

## 장점

1. 상태를 유지하지 않고(Stateless), 확장에 용이한(Scalable) 애플리케이션을 구현하기 용이하다.
2. 여러대의 서버를 이용하는 서비스라면 하나의 토큰으로 여러 서버에서 인증이 가능하다. 만약 세션 방식이라면 모든 서버가 해당 사용자의 세션 정보를 공유하고 있어야 한다.
3. 클라이언트가 request를 전송할 때마다 자격 증명 정보를 전송할 필요가 없다. (토큰의 유효기간이 만료되기 전까지는 한 번의 인증만 수행하면 된다.)
4. 인증을 담당하는 시스템을 다른 플랫폼으로 분리하는 것이 용이하다. (Github, Goolge 등의 플랫폼의 자격 증명 정보로 인증하는 것이 가능하다.)
5. 권한 부여에 용이하다. 토큰의 Payload 안에 해당 사용자의 권한 정보를 포함시키면 된다.

## 단점

1. Payload는 디코딩이 용이하기 때문에 토큰을 탈취하여 저장한 데이터를 확인하기 쉽다. 따라서 민감한 정보를 포함시키지 않아야 한다. 
2. 토큰의 길이가 길어지면 네트워크에 부하를 줄 수 있다. 
3. 토큰은 자동으로 삭제되지 않기 때문에 토큰 만료 시간을 반드시 추가해야 하고, 토큰이 탈취될 경우를 대비해서 토큰의 유효 기간을 너무 길게 설정하지 않아야 한다. 

## JWT의 종류

- JWT는 두 가지 종류의 토큰을 사용자의 자격 증명에 이용해야 한다.
    - 액세스 토큰(Access Token)
    - 리프레시 토큰(Refresh Token)
- 클라이언트가 처음 인증을 받게 될 때(로그인 시), Access Token과 Refresh Token 두 가지를 다 받지만, 실제로 권한을 얻는 데 사용하는 토큰은 Access Token이다.

### 액세스 토큰(Access Token)

- 보호된 정보들(사용자의 이메일, 연락처, 사진 등)에 접근할 수 있는 권한 부여에 사용한다.
- Access Token은 탈취되더라도 오랫동안 사용할 수 없도록 비교적 짧은 유효 기간을 주는 것을 권장한다.
- Base64로 인코딩되는 Payload는 손쉽게 디코딩이 가능하기 때문에 민감한 정보는 포함하지 않아야 한다.

### 리프레시 토큰(Refresh Token)

- Access Token의 유효기간이 만료된다면 Refresh Token을 사용하여 새로운 Access Token을 발급받는다. 이 때, 사용자는 다시 로그인 인증을 할 필요가 없다.
- Refresh Token까지 탈취당할 수 있기 때문에 편의보다 정보 보안이 중요한 웹 애플리케이션은 Refresh Token을 사용하지 않는 곳도 많다.

## JWT 구조

```java
[Header].[Payload].[Signature]
```

- JWT는 `.` 으로 나누어진 세 부분이 존재한다.

### Header

- Header는 어떤 종류의 토큰인지, 어떤 알고리즘으로 Sign할 것인지 정의한다.
- JSON 포맷 형태로 정의한다.
- 이 JSON 객체를 base64 방식으로 인코딩하면 JWT의 Header 부분이 완성된다.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

- Payload는 서버에서 활용할 수 있는 사용자의 정보가 담겨 있다.
- 정보 접근에 대한 권한을 담을 수 있고, 사용자의 이름 등 필요한 데이터를 담을 수 있다.
- Payload도 Base64로 인코딩되지만 손쉽게 디코딩이 가능하므로 민감한 정보는 포함하지 않아야 한다.
- JSON 객체를 base64로 인코딩하면 Payload 부분이 완성된다.

```json
{
  "sub": "someInformation",
  "name": "phillip",
  "iat": 151623391
}
```

### Signature

- base64로 인코딩 된 Header, Payload 부분이 완성되었다면 , Signature에서는 원하는 Secret Key와 Header에서 지정한 알고리즘을 사용하여 Header와 Payload에 대해서 단방향 암호화를 수행한다.
- 암호화 된 메시지는 토큰의 위변조 유무를 검증하는데 사용된다.
- 예를 들어, HMAC SHA256 알고리즘을 사용한다면 Signature는 아래와 같은 방식으로 생성된다.

```json
HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), secret);
```

## 토큰 기반 인증 절차

1. 클라이언트가 서버에 아이디/비밀번호를 담아 로그인 요청을 보낸다.
2. 아이디/비밀번호가 일치하는지 확인하고, 클라이언트에게 보낼 암호화 된 Access Token과 Refresh Token을 생성한다. 
    - 토큰에 담길 정보(Payload)는 사용자를 식별할 정보, 사용자의 권한 정보 등이 될 수 있다.
    - Refresh Token을 이용해 새로운 Access Token을 생성할 것이므로 두 종류의 토큰이 같은 정보를 담을 필요는 없다.
3. 토큰을 클라이언트에게 전송하면, 클라이언트는 토큰을 저장한다.(Local Storage, Session Stroage, Cookie 등에 저장)
4. 클라이언트가 HTTP Header(Authorization Header) 또는 쿠키에 토큰을 담아 request를 전송한다. (Bearer authentication을 이용한다.)
5. 서버는 토큰을 검증해서 클라이언트의 요청을 처리한 후 응답을 보내준다. 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3659e936-a853-457e-9f8d-2443704789b8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221123T070951Z&X-Amz-Expires=86400&X-Amz-Signature=7fbcce961fda290f9a973094b5b7ecad7712974468eef226079a1bca8134f6a4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)