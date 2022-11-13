* [Asciidoc](#mockito)
    + [Asciidoc이란?](#asciidoc이란)
    + [목차 구성](#목차-구성)
    + [박스 문단 사용하기](#박스-문단-사용하기)
    + [URL Scheme 자동 인식](#url-scheme-자동-인식)
    + [이미지 추가](#이미지-추가)
    + [문서 스니핏을 템플릿 문서에 포함시키기](#문서-스니핏을-템플릿-문서에-포함시키기)
    + [핵심 포인트](#핵심-포인트)

# Asciidoc

## Asciidoc이란?

> Asciidoctor는 AsciiDoc 포맷의 문서를 파싱해서 HTML 5, 매뉴얼 페이지, PDF 및 EPUB 3 등의 문서를 생성하는 툴이다.
> 
- Spring Rest Docs에서는 Asciidoc 포맷의 문서를 HTML 파일로 변환하기 위해 내부적으로 Asciidoctor를 사용하고 있다.
- Asciidoc 포맷을 사용해서 메모, 문서, 기사, 서적, E-Book, 웹 페이지, 메뉴얼 페이지, 블로그 게시물 등을 작성할 수 있다.
- Asciidoc 포맷으로 작성된 문서는 HTML, PDF, EPUB, 메뉴얼 페이지를 포함한 다양한 형식으로 변환될 수 있다.
- Asciidoc은 주로 기술 문서 작성을 위해 설계된 가벼운 마크업 언어이기도 하다.
- Spring Rest Docs를 통해 만들어지는 문서 스니핏과 이 스니핏을 사용하는 템플릿 문서는 Asciidoc 포맷의 문서로 이루어져 있기 때문에 API를 사용하는 이들이 직관적으로 이해할 수 있는 API 문서를 만들기 위해 Asciidoc 기본 문법은 알고 있는 것이 좋다.

## 목차 구성

```html
= 커피 주문 애플리케이션
:sectnums:
:toc: left
:toclevels: 4
:toc-title: Table of Contents
:source-highlighter: prettify

Chan Bin Kim <gksmfcksqls@gmail.com>

v1.0.0, 2002.07.10
```

- `=` : 문서의 제목을 작성하기 위해서는 `=` 를 추가하면 된다. `====` 와 같이 `=` 의 개수가 늘어날수록 글자는 작아진다.
- `:sectnums:` : 목차에서 각 섹션에 넘버링을 해준다.
- `:toclevels:` : 목차에 표시할 제목의 level을 지정한다. 여기서는 4로 지정했기 때문에 `====` 까지의 제목만 목차에 표시된다.
- `:toc-title:` : 목차의 제목을 지정할 수 있다.
- `:source-highlighter:` : 문서에 표시되는 소스 코드 하이라이터를 지정한다. 여기서는 `prettify` 를 지정했다.
    
    ![스크린샷 2022-11-12 오후 5.18.23.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ceaa44d5-baa6-4172-bcf1-bb402cad550b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-11-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.18.23.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221113T055209Z&X-Amz-Expires=86400&X-Amz-Signature=14810c5902c01d28e77b1f863dc2eefc3c0c0d9ed392d3b19e0d38a965ba908a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-11-12%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25205.18.23.png%22&x-id=GetObject)
    

## 박스 문단 사용하기

```html
***
API 문서 개요

	이 문서는 Spring MVC 기반의 REST API 기반 샘플 애플리케이션입니다.

CAUTION: 이 문서는 일부 기능에 제한이 있습니다. 기능 제한 사항에 대해 알고 싶다면 담당자에게 문의 하세요.
***
```

- `***` : 단락을 구분지을 수 있는 수평선을 추가해준다.
- 문단의 제목 다음에 한 라인을 띄우고 한 칸 들여쓰기를 하면 박스 문단을 사용할 수 있다.
- `CAUTION:` : 경고 문구를 추가할 수 있다. 이 외에 `NOTE:`, `TIP:` ,  `IMPORTANT:` , `WARNING:` 등을 사용할 수 있다.

![스크린샷 2022-11-12 오후 5.36.32.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/762bb076-d74c-4095-a957-1e3b0f8338c7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-11-12_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.36.32.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221113T055222Z&X-Amz-Expires=86400&X-Amz-Signature=995fbdad190a1bb5aba2e35fb5582e4df269fbd26eb83725b3d1fd3141b2434a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-11-12%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25205.36.32.png%22&x-id=GetObject)

## URL Scheme 자동 인식

- 다음과 같은 URL Scheme는 Asciidoc에서 자동으로 인식하여 링크가 설정된다.
    - http
    - https
    - ftp
    - irc
    - mailto
    - hgd@gmail.com

## 이미지 추가

- API 문서에 이미지를 추가하고 싶다면 `image::` 를 사용해서 추가할 수 있다.

```html
image::https://spring.io/images/spring-logo-9146a4d3298760c2e7e49595184e1975.svg[spring]
```

## 문서 스니핏을 템플릿 문서에 포함시키기

- 테스트 케이스 실행을 통해 생성된 스니핏(snippet)을 템플릿 문서(index.adoc)에 포함(inclulde)시키는 방법을 알아보자
- 템플릿 문서에 포함된 스니핏은 애플리케이션 빌드 타임에 내부적으로 Asciidoctor가 index.adoc를 index.html로 변환 후, 특정 디렉토리 `src/main/resources/static/docs` 에 생성해준다.

```html
***
== MemberController
=== 회원 등록
.curl-request     
include::{snippets}/post-member/http-request.adoc[] 

.request-fields
include::{snippets}/post-member/request-fields.adoc[]

.http-response
include::{snippets}/post-member/http-response.adoc[]

.response-fields
include::{snippets}/post-member/response-fields.adoc[]

...
...
```

- `.curl-request` 에서 `.` : 하나의 스니핏 섹션 제목을 표현하기 위해 사용한다. `curl-request` 는 섹션의 제목이며, 원하는 대로 수정하면 된다
- `include` 는 Asciidoctor에서 사용하는 매크로(macro) 중 하나이며, 스니핏을 템플릿 문서에 포함할 때 사용한다.
- `::` 은 매크로를 사용하기 위한 표기법이다.
- `{snippets}` 는 해당 스니핏이 생성되는 디폴트 경로를 의미하며, bulid.gradle 파일에 설정한 `snippetsDir` 변수를 참조하는데 사용할 수 잇다.

```html
ext {
		set('snippetsDir', file("build/generated-snippets"))
}
...
...
```

- Asciidoctor에서는 어떤 작업을 처리하기 위한 용어로 매크로(macro)라는 용어를 사용한다.
- 매크로(macro)는 일반적으로 어떤 반복되는 작업을 자동화하는 의미를 가지며, 우리가 흔히 알고 있는 매크로에는 엑셀 등의 스프레드시트에서 사용할 수 있는 매크로 기능이 있다.

## 핵심 포인트

- Asciidoc은 Spring Rest Docs를 통해 생성되는 테긋트 기반 문서 포맷이다.
- Asciidoc은 주로 기술 문서 작성을 위해 설계된 가벼운 마크업 언어이기도 한다.
- Asciidoc을 이용해서 조금 더 세련되고, 가독성 좋은 API 문서를 만들 수 있다.
- Asciidoctor는 Asciidoc 포맷의 문서를 파싱해서 HTML5, 매뉴얼 페이지, PDF 및 EPUB3 등의 문서를 생성하는 툴이다.