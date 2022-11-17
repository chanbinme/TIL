* [CSRF(Cross Site Request Forgery)](#csrfcross-site-request-forgery)
    + [CSRF 공격을 하기 위한 조건](#csrf-공격을-하기-위한-조건)
    + [GET 요청으로 CSRF 공격하기](#get-요청으로-csrf-공격하기)
    + [CSRF는 어떻게 막을 수 있을까?](#csrf는-어떻게-막을-수-있을까)

# CSRF(Cross Site Request Forgery)

> 다른 사이트(cross-site)에서 유저가 보내는 요청(request)를 조작(Forgery)하는 것
> 
- 예를 들어 이메일에 첨부된 링크를 누르면 내 은행계좌의 돈이 빠져나가는 경우 공격자가 내 은행 계좌로 요청을 보내서 돈을 빼 가는 방식의 해킹
- 다른 오리진이기 때문에 response에 직접 접근할 수 없다.

## CSRF 공격을 하기 위한 조건

- 쿠키를 사용한 로그인 : 유저가 로그인했을 때, 쿠키로 어떤 유저인지 알 수 있어야 한다.
- 예측할 수 있는 요청/parameter를 가지고 있어야 한다. request에 해커가 모를 수 있는 정보가 담겨있으면 안된다.

## GET 요청으로 CSRF 공격하기

1. 유저가 웹사이트에 로그인을 했다.(유저의 정보가 쿠키에 담겨있음)
2. 공격자는 돈을 보낼 때 사용되는 GET요청에서 유저의 계좌번호를 공격자의 계좌 번호로 바꾼다.
    
    `<a href="https://bank/transfer?account_number=유저계좌번호&amount=10000000W">` → `<a href="https://bank/transfer?account_number=공격자 계좌번호&amount=10000000W">`
    
3. 공격자는 유저가 CSRF 공격을 위한 악성 링크를 클릭하도록 유도한다.
4. 유저가 악성 링크를 클릭하면 공격자는 유저의 계정으로 공격자의 계좌로 돈을 보내도록 만든다.

## CSRF는 어떻게 막을 수 있을까?

- CSRF 토큰 사용하기 : 서버측에서 CSRF 공격에 보호하기 위한 문자열을 유저의 브라우저와 웹 앱에만 제공
- Same-site cookie 사용하기 : 같은 도메인에서만 세션/쿠키를 사용할 수 있다.