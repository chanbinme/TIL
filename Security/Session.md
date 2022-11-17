* [Session](#session)
    + [Cookie와 Session의 차이점](#cookie와-session의-차이점)
    + [세션기반 인증(Session-based Authentication)](#세션기반-인증session-based-authentication)
    + [로그아웃](#로그아웃)

# Session

- 서버가 Client에 유일하고 암호화된 ID를 부여
- 중요 데이터는 서버에서 관리

## Cookie와 Session의 차이점

|  | 설명 | 접속 상태 경로 | 장점 | 단점 |
| --- | --- | --- | --- | --- |
| Cookie | 쿠키는 그저 http의 statless한 것을 보완해주는 도구 | 클라이언트 | 서버의 부담을 덜어줌 | 쿠키 그 자체는 인증이 아님 |
| Session | 접속 상태를 서버가 가짐(stateful)
접속 상태와 권한 부여를 위해 세션 아이디를 쿠키로 전송 | 서버 | 신뢰할 수 있는 유저인지 서버에서 추가로 확인 가능 | 하나의 서버에서만 접속 상태를 가지므로 분산에 불리 |

## 세션기반 인증(Session-based Authentication)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f709c041-1bc9-431b-9ae8-472602a47d2c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221117T122804Z&X-Amz-Expires=86400&X-Amz-Signature=89d9ff937fefda208b0741d1d46e0cef8be4d9ede9992f3ea6823237a6222c8e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 사용자가 웹사이트에서 아이디 및 비밀번호를 이용해서 로그인에 성공하면, 서버는 인증(Authentication)에 성공했다고 판단한다.
- 이후 인증을 필요로 하는 작업을 요청할 경우, 서버는 해당 사용자가 인증에 성공했음을 알고 있기 때문에 유저는 매번 로그인할 필요가 없다. 인증에 따라 리소스의 접근 권한(Autorization)이 달라진다.
- 서버와 클라이언트에 각각 필요한 것은 다음과 같다.
    - 서버 : 사용자가 인증에 성공했음을 알고 있어야 한다.
    - 클라이언트 : 인증 성공을 증명할 수단을 갖고 있어야 한다.
- 사용자가 인증에 성공한 상태는 세션이라고 부른다.
    - 서버는 일종의 저장소에 세션을 저장한다. 주로 in-memory, 또는 세션 스토어(redis 등과 같은 트랜잭션이 빠른 DB )에 저장한다.
- 세션이 만들어지면, 각세션을 구분할 수 있는 세션 아이디도 만들어지는데, 보통 클라이언트에 세션 성공을 증명할 수단으로써 세션 아이디를 전달한다.
- **이 때 웹사이트에서 로그인을 유지하기 위한 수단으로 쿠키를 사용한다. 쿠키에는 서버에서 발급한 세션 아이디를 저장한다.**
- 쿠키를 통해 유효한 세션 아이디가 서버에 전달되고, 세션 스토어에 해당 세션이 존재한다면 서버는 해당 요청에 접근 가능하다고 판단한다.
- 하지만 쿠키에 세션 아이디 정보가 없는 경우, 서버는 해당 요청이 인증되지 않았음을 알려준다.

## 로그아웃

- 세션 아이디가 담긴 쿠키는 클라이언트에 저장되어 있으며, 서버는 세션을 저장하고 있다. 그리고 서버는 그저 세션 아이디로만 인증 여부를 판단한다.

> ⚠️ 주의 : 쿠키는 세션 아이디, 즉 인증 성공에 대한 증명을 갖고 있으므로, 탈취될 경우 서버는 해당 요청이 인증된 사용자의 요청이라고 판단한다. 이것이 우리가 공공 PC에서 로그아웃을 해야 하는 이유이다.
> 
- 로그아웃은 두 가지 작업을 해야 한다.
    - 서버 : 세션 정보를 삭제해야 한다.
    - 클라이언트 : 쿠키를 갱신해야 한다.
- 서버가 클라이언트의 쿠키를 임의로 삭제할 수는 없다. 대신, `set-cookie` 로 클라이언트에게 쿠키를 전송할 때 세션 아이디의 키값을 무효한 값으로 갱신할 수 있다.