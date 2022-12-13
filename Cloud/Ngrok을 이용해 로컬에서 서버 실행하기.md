# ****Ngrok을 이용해 로컬에서 서버 실행하기****

## ****Ngrok 이란?****

- Ngrok은 네트워크 설정을 하지 않아도 방화벽을 넘어 외부에서 로컬 환경에 접근할 수 있게 해주는 터널링 프로그램이다.
- 무료 플랜의 경우 연결 세션이 약 2시간가량 유지되며 개발 목적으로 임시 도메인을 발급받아 테스팅하기에 유용하다. (AuthToken 등록시 시간 제한 없이 사용 가능)
- 개발 영역이 나누어져있는 환경에서 통신테스트 할 때 유용하게 사용할 수 있다.

## Ngrok 설치(Mac 기준)

[Ngrok 공식 홈페이지 Download 탭](https://ngrok.com/download)

(1) Zip 파일을 다운로드 한다.

(2) 터미널에 명령어를 복사 붙여넣기 한다.

이후 터미널에서 `ngrok -h` 명령어를 입력했을 때 ngrok 명령어 옵션에 대한 설명이 나온다면 정상적으로 설치 되었음을 알 수 있다.

## ****Ngrok 실행****

```
# ngrok http {port} 의 형태로 원하는 포트를 연결할 수 있다.
$ ngrok http 8080
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/856abf46-3c56-415a-be0b-261766fdf1dc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221213%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221213T070821Z&X-Amz-Expires=86400&X-Amz-Signature=2fde3979e870932f7a4f4ea654685f318044f0a6b3b1a9c923fdef03a474dde3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- ngrok 실행 시 위의 이미지와 같은 화면이 뜨며 8080번 포트로 포워딩하는 임시 도메인과, 해당 연결의 세션 지속시간 등을 확인할 수 있다.
- 톰캣 서버를 실행한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c4d16a9e-d4ee-4f5c-ad9c-c83ff99d9ac8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221213%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221213T070831Z&X-Amz-Expires=86400&X-Amz-Signature=2feb953ba1955111c0c9e33333f07622360c24681724256ee795917f115b44d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 요청 주소를 `http://localhost:8080`이 아닌, Ngrok이 안내한 임시 도메인으로 설정하면 된다.
- Ngrok을 멈춘 후 다시 시작하면 임시 도메인 주소가 바뀌니 항상 확인할 것
- 이제 다른 사람의 컴퓨터에서도 내 로컬 환경에 접근할 수 있게 되었습니다.

## **Token 등록**

- Ngrok 홈페이지에서 회원가입 후 발급되는 Auth Token을 등록할 수 있습니다. 등록 후 다음과 같은 서비스를 추가 이용 가능하다.
- 1회 세션 연결 지속시간이 2시간에서 24시간으로 늘어난다.
- 토큰 등록 전엔 웹 브라우저를 통해 임시 도메인에 연결 시 HTML이 보이지 않습니다. 토큰 등록 후 이용 가능하다.