* [OAuth2](#oauth2)
    + [OAuth 2를 사용하는 애플리케이션 유형](#oauth-2를-사용하는-애플리케이션-유형)
* [OAuth2의 동작 방식](#oauth2의-동작-방식)
    + [OAuth2 인증 컴포넌트들의 역할](#oauth2의-동작-방식)
    + [OAuth2 인증 처리 흐름](#oauth2-인증-처리-흐름)
    + [OAuth2 인증 프로토콜에서 사용되는 용어](#oauth2-인증-프로토콜에서-사용되는-용어)
    + [Authorization Grant 유형](#authorization-grant-유형)
    + [정리](#정리)


# OAuth2

> OAuth 2는 특정 애플리케이션에서 사용자의 인증을 처리하는 것이 아니라 사용자 정보를 보유하고 있는 신뢰할 만한 써드 파티 애플리케이션(GitHub, Goolge, Facebook 등)에서 사용자의 인증을 대신 처리해주고 Resource에 대한 자격 증명용 토큰을 발급한 후, Client가 해당 토큰을 이용해 써드 파티 애플리케이션의 서비스를 사용하게 해주는 방식이다.
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a437e3e6-51fb-4012-8b2e-f9309137d7ab/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081204Z&X-Amz-Expires=86400&X-Amz-Signature=10ce2b70c48dd9f37798208cc025d114ecbcad9cd76dab5bb4f8f546397b2450&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 애플리케이션은 사용자의 Google 전용 크리덴셜(Credential)를 저장할 필요가 없기 때문에 사용자의 크리덴셜을 이중으로 관리하지 않아도 된다.

## OAuth 2를 사용하는 애플리케이션 유형

### 써드 파티 애플리케이션에서 제공하는 API의 직접적인 사용

- Google, Github, Facebook 같은 신뢰할만한 써드 파티 애플리케이션에서 제공하는 API를 직접적으로 사용하는 애플리케이션을 구현하는 데 OAuth 2를 사용할 수 있다.
- 사용자가 OAuth 2 인증 프로토콜을 이용해 써드 파티 애플리케이션에 대한 인증에 성공하면 써드 파티 애플리케이션에서 제공하는 API를 활용한 커스텀 서비스를 제공하는 것이다.

### 추가적인 인증 서비스 제공 용도

- 일반적으로 제공하는 아이디/패스워드 로그인 인증 이외에 OAuth 2를 이용한 로그인 인증 방법을 추가적으로 제공한다.
- 만약 특정 서비스를 제공하는 애플리케이션에서 사용자의 크리덴셜(Credential)을 남기고 싶지 않을 경우 OAuth 2 로그인 인증 방법으로 로그인하면 된다.

# OAuth2의 동작 방식

## OAuth2 인증 컴포넌트들의 역할

### Resource Owner

- 사용하고자 하는 Resource의 소유자를 말한다.
- `Resource Owner` 는 한마디로 Google 등의 서비스를 이용하는 사용자이다.
- Bin이라는 사용자가 구글 계정으로 로그인해서 Google의 서비스를 이용하고 있다면 Bin이 Google 서비스라는 Resource에 대한 Resource Owner가 된다.

### Client

- Resource Owner를 대신해 보호된 Resource에 엑세스하는 애플리케이션이다.
- Client는 서버, 데스크탑, 모바일 또는 기타 장치에서 실행되는 애플리케이션이 될 수 있다. “어떤 서비스를 이용하고자 하는 쪽은 Client”라고 생각하면 좋다.
- Bin이라는 사용자가 A라는 애플리케이션을 통해서 Google의 소셜 로그인을 이용한다면 애플리케이션 A가 Client이다.

### Resource Server

- OAuth2 인증 처리 흐름에서 `Resource Server` 는 **Client의 요청을 수락하고 Resource Owner에게 해당하는 Resource를 제공하는 서버이다.**
- A라는 애플리케이션(Client)이 Google Photo에서 Resource Owner의 사진(Resource)을 가져오는 경우, Google Photo 서비스를 제공하는 애플리케이션이 `Resource Server` 가 된다.

### Authorization Server

- OAuth2 인증 처리 흐름에서 `Authorization Server` 는 **Client가 Resource Server에 접근할 수 있는 권한을 부여하는 서버이다.**
- Resource Owner가 인증에 성공하면 `Authorization Server` 는 Client에게 Access Token 형태로 Resource Owner의 Resource에 접근할 수 있는 권한을 부여한다.
- Bin이라는 사용자(Resource Owner)가 구글 로그인 인증에 성공하면 애플리케이션A(Client)가 `Authorization Server` 로부터 Google Photo에 저장되어 있는 Bin의 사진(Resource)에 접근할 수 있는 권한(Access Token)을 부여 받는다.

## OAuth2 인증 처리 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/580e7a3c-45b6-4f09-bb51-a88fc647bf43/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081220Z&X-Amz-Expires=86400&X-Amz-Signature=4afa15e8e58a6a7b388d0f59a76561eb3f646d28b711c9959e65822eea93daee&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. Resource Owner는 웹 애플리케이션(Client)에게 OAuth2 인증을 요청한다.
    
    여기서 인증 요청은 Client인 웹 애플리케이션이 자체적으로 로그인 화면을 제공하는 것이 아니다. 
    
    Resource Owner는 자신의 계정 정보를 관리하고 있는 써드 파티 애플리케이션에 로그인하길 원하는 것이며, 이 요청을 Client인 웹 애플리케이션에 전송하는 것이다. 
    
2. Client는 Resource Owner가 써드 파티 애플리케이션에 로그인 할 수 있도록 써드 파티 애플리케이션의 로그인 페이지로 리다이렉트(Redirect)한다.
3. Resource Owner는 로그인 인증을 진행하고 로그인 인증에 성공하면
4. 🔥(중요) Authorization Server가 Access Token을 Client에게 전송한다. 
    
    Resource Owner에게 Access Token을 전송하는 것이 아니라 Client 애플리케이션에게 전송하는 것임을 꼭 기억해두자.
    
    Client가 Resource Owner의 Resource를 사용하는 대리인 역할을 하기 때문이다.
    
5. Access Token을 전달 받은 Client는 Resource Server에게 Resource Owner 소유의 Resource를 요청한다.
6. Resource Server는 Client가 전송한 Access Token이 검증되면 Resource Owner의 Resource를 Client에게 전송한다. 
- 핵심 : Resource를 소유하고 있는 Resource Owner를 대신하는 Client가 Resource Owner의 대리인 역할을 수행한다.

## OAuth2 인증 프로토콜에서 사용되는 용어

### Authorization Grant

- Authorization Grant는 **Client 애플리케이션이 Access Token을 얻기 위한 Resource Owner의 권한을 표현하는 크리덴셜을 의미**한다.
- Autorization Grant는 네 가지 타입이 있다.
    - Autorization Code
    - Implicit Grant Type
    - Client Credentials
    - Resource Owner Password Credentials

### Access Token

- Access Token은 Client가 Resource Server에 있는 보호된 Resource에 액세스하기 위해 사용하는 자격 증명용 토큰이다.
- Authorization Code와 Client Secret을 이용해 Authorization Server로부터 전달 받은 Access Token으로 Resource Server에 접근할 수 있다.

### Scope

- Scope는 주어진 액세스 토큰을 사용하여 액세스 할 수 있는 Resource의 범위를 의미한다.
- 예를 들어 Scope 설정을 통해 Resource Owner의 사진첩에는 접근할 수 있지만, 연락처 등 다른 Resource에는 접근할 수 없도록 접근 권한을 지정할 수 있다.

## Authorization Grant 유형

### Authorization Code Grant : 권한 부여 승인 코드 방식

- Authorization Code Grant는 권한 부여 승인을 위해 자체 생성한 Authorization Code를 전달하는 방식으로, 가장 많이 쓰이는 기본 방식이다.
- Authorization Code Grant 방식은 Refresh Token을 사용할 수 있다.
- 권한 부여 승인 요청시 응답 타입(response_type)을 code로 지정하여 요청한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b7a7f456-dab4-451a-b1d0-d5f224eff67b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081229Z&X-Amz-Expires=86400&X-Amz-Signature=5b7dc84b311b3fa0ed5917d7b6502973370cfd84fb1b17c2bff1902ed91ac642&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. Resource Owner는 서비스 요청(로그인 등)을 애플리케이션(Client)에게 전송한다.
2. Client는 Authorization Server에 Authorization Code를 요청한다. (이 때, 미리 생성한 Client ID, Redirect URI, 응답 타입을 함께 전송한다.)
3. Resource Owner는 로그인 페이지를 통해 로그인을 진행한다. 
4. 로그인이 되면 Authorization Server는 Authorization Code를 Client에게 전달한다.(이전에 요청과 함께 전달한 Redirect URI로 Code를 전달한다.)
5. Client는 전달받은 Authorization Code를 이용해 Access Token 발급을 요청한다. (이 때 미리 생성한 Clinet Secret, Redirect URI, 권한 부여 방식, Authroziation Code를 함께 전송한다.)
6. 요청 정보를 확인한 후 Redirect URI로 Access Token을 발급한다.
7. Client는 발급 받은 Access Token을 이용해 Resource Server에 Resource를 요청한다.
8. Resource Server는 Access Token을 확인한 후 요청 받은 Resource를 Client에게 전달한다.

### Implict Grant : 암묵적 승인 방식

- **별도의 Authroization Code 없이 바로 Access Token을 발급하는 방식**
- 자격증명을 안전하게 저장하기 힘든 Client(자바스크립트 등 스크립트 언어를 사용하는 브라우저)에게 최적화된 방식이다.
- Refresh Token 사용이 불가능하며, Authorization Server는 Client Secret을 통해 클라이언트 인증 과정을 생략한다.
- 권한 부여 승인 요청시 응답 타입(response_type)을 token으로 지정하여 요청한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/bd29fd3d-d83d-4e3f-aad3-b2df2518cf57/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081243Z&X-Amz-Expires=86400&X-Amz-Signature=ac77214d85a9d91b502b40fc61ef4e7faa6e843ced701f6095c24b9eac4f24e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. Resource Owner는 서비스 요청(로그인 등)을 애플리케이션(Client)에게 전송한다.
2. Client는 Authorization Server에게 접근 권한을 요청한다. (이 때, 미리 생성한 Client ID, Redirect URI, 응답 타입을 함께 전송한다.)
3. Resource Owner는 로그인 페이지를 통해 로그인을 진행한다. 
4. 로그인이 되면 요청 정보를 확인한 후 Authorization Server는 Client에게 Access Token을 전달한다.
5. Client는 발급 받은 Access Token을 이용해 Resource Server에 Resource를 요청한다.
6. Resource Server는 Access Token을 확인한 후 요청 받은 Resource를 Client에게 전달한다.

### Resource Owner Password Credential Grant : 자원 소유자 자격 증명 승인 방식

- 로그인 시 필요한 정보(username, password)로 Access Token을 발급 받는 방식이다.
- 자신의 서비스에서 제공하는 애플리케이션의 경우에만 사용되는 인증 방식으로, Refresh Token의 사용도 가능하다.
- 예를 들어 네이버 계정으로 네이버 웹툰 애플리케이션에 로그인, 카카오 계정으로 카카오 지도 애플리케이션에 로그인하는 경우가 Resource Owner Password Credential Grant에 해당한다.
- **Authentication Server, Resource Server, Client가 모두 같은 시스템에 속해 있을 때 사용이 가능하다.**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/359b4ed5-2ced-4fe6-98aa-6f50c0381f94/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081254Z&X-Amz-Expires=86400&X-Amz-Signature=b1892adf6635cb0941f230f2a6e387a25bbb4e663b5b59514947b16ddfe80cb9&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. Resource Owner는 서비스 요청(로그인 등)을 애플리케이션(Client)에게 전송한다.
2. Client는 Resource Owner에게 전달 받은 로그인 정보를 통해 Authorization Server에 Access Token을 요청한다. (이 때, 미리 생성한 Client ID, 권한 부여 방식, 로그인 정보를 함께 전달한다.)
3. 요청과 함께 온 정보들을 확인한 후 Client에게 Access Token을 전달한다.
4. Client는 발급 받은 Access Token을 이용해 Resource Server에 Resource를 요청한다.
5. Resource Server는 Access Token을 확인한 후 요청 받은 Resource를 Client에게 전달한다.

### Client Credentials Grant : 클라이언트 자격 증명 승인 방식

- Client가 관리하는 Resource 혹은 Authorization Server에 해당 Client를 위한 제한된 Resource 접근 권한이 설정되어 있는 경우 사용 가능한 방식이다.
- 이 방식은 자격 증명을 안전하게 보관할 수 있는 Client에서만 사용되어야 하며, Refresh Token의 사용은 불가능하다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3dc21e47-7aa4-4f6b-a45d-2c428525d0c4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221125T081306Z&X-Amz-Expires=86400&X-Amz-Signature=9fd13fe47613bc3da103037af37b289f014d7f2f1e38450885f26d358096ac20&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

## 정리

- OAuth2 인증 컴포넌트
    - `Resource Owner` : 사용하고자 하는 Resource의 소유자
    - `Client` : Resource Owner를 대신해 보호된 Resource에 액세스하는 애플리케이션
    - `Resource Server` : Client의 요청을 수락하고 Resource Owner에 해당하는 Resource를 제공하는 서버
    - `Authorization Server` : Client가 Resource Server에 접근할 수 있는 권한(Access Tokne)을 부여하는 서버
- OAuth2 인증 프로토콜의 핵심은 **어떤 Resource를 소유하고 있는 Resource Owner를 대신하는 누군가(Client)가 Resource Owner의 대리인 역할을 수행한다는 것**이다.
- Authorization Grant에 따른 인증 처리 방식
    - Authorization Code Grant : 권한 부여 승인 코드 방식
    - Implict Gratn : 암묵적 승인 방식
    - Resource Owner Password Credential Grant : 자원 소유자 자격 증명 승인 방식
    - Client Credential Grant : 클라이언트 자격 증명 승인 방식