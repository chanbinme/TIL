# 목차
* [목차](#목차)
* [HTML](#html)
    + [기본구조](#html-기본-구조)
    + [문법](#html-문법)
    + [마크업 언어 개념](#마크업-언어-개념)
* [HTML 요소](#html-요소)
    + [div](#div-vs-span)
    + [span](#div-vs-span)
    + [img](#img)
    + [a](#a)
    + [ul/ol](#ul--ol)
    + [input](#input)
* [시멘틱 태그 개념](#시멘틱-태그-개념)
</br>
</br>

# HTML
<pre>
HTML(Hyper Text Markup Language)은 웹 페이지의 구조를 표현하는 마크업 언어다.
</pre>
## HTML 기본 구조

```html
<!DOCTYPE html>                     
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h1>Hello World!<h1>
        <div>Contents Here
            <span>Here Too!</span>
        </div>
    </body>
</html>
```
* 트리구조(Tree Structure)라고도 불린다. 트리는 하나의 시작점으로 부터 뻗어나온 자식노드의 집합이라고 볼 수 있다.
* HTML 문서의 시작은 `<html>` 태그로 시작하고 `</html>` 태그로 끝난다.
* HTML 문서 내부는 `<head>` 태그와 `<body>`태그로 이루어져 있다.
## HTML 문법
* HTML은 태그(tag)들의 집합이다.
* 태그는 시작 태그(start tag)와 종료 태그(end tag)의 한 쌍으로 구성된다.
* 종료 태그는 시작 태그와 똑같지만, 태그 이름 앞에 슬래스(/)가 존재한다.
* HTML은 꺽새괄호(<>)와 태그(Tag), 속성(Attribute), 값(Arguments)을 이용하여 표현한다.
```html
<태그 속성="값"></태그>

<style type="text/css></style>
```
* `<img>`와 같이 종료 태그 없이 시작 태그만을 가지는 태그를 빈 태그(empty tag)라고 한다.
* 빈 태그는 시작 태그와 종료 태그를 한 번에 쓸 수 있다.
```html
<태그 속성="값" />

<img src="chanbin-logo.png" />
```
## 마크업 언어 개념
```
마크업 언어(Markup Language)는 태그 등을 이용하여 문서나, 데이터의 구조를 표현하는 언어다.
```
* 프로그래밍 언어와는 구별된다.
</br>
</br>

# HTML 요소
<!-- Markdown -->
태그|설명
-|-
`<div>`|Division
`<span>`|Span
`<img>`|Image
`<a>`|Link
`<ul>` & `<ol>`|Unordered List & Ordered List
`<input>`|Input (Text, Radio, Checkbox)
`<textarea>`|Multi-line Text Input
`<button>`|Button

## `<div>` vs. `<span>`
* `<div>` 태그는 한 줄을 차지한다.
* `<span>` 태그는 컨텐츠 크기만큼 공간을 차지한다.
* 스타일 지정 목적 또는 스크립팅의 편의를 위해 사용하는 것이 좋다.

## `<img>`
* 이미지 삽입 태그
* 닫는 태그가 없다.
* src 속성은 필수이며, 포함하고자 하는 이미지의 경로를 지정한다.
* alt 속성은 이미지의 텍스트 설명이며 필수는 아니지만, 스크린 리더가 alt의 값을 읽어 사용자에게 이미지를 설명하므로, 접근성 차원에서 매우 유용하다. 또한 네트워크 오류, 콘텐츠 차단, 죽은 링크 등 이미지를 표시할 수 없는 경우에도 이 속성의 값을 대신 보여준다.
```html
<img src="https://i.imgur.com/JVAdfdf.jpg" alt="이쁜 사진">
```
## `<a>`
* 링크 삽입
* anchor(닻; 배의 위치를 고정)의 약자
* 다른 웹페이지로 연결되는 하이퍼링크를 HTML문서에 표시할 때 사용
* href 속성과 사용
```html
<a href="https://naver.com">네이버</a>
```
## `<ul>` & `<ol>`
* 리스트
* ul(unordered list): 순서가 없는 리스트
* ol(ordered list): 순서가 있는 리스트
```html
<ul>
    <li>item1</li>
    <li>item2</li>
    <li>item3</li>
    <ol>
        <li>item4</li>
        <li>item5</li>
    </ol>
</ul>
```
> * item1
> * item2
> * item3 </br>
> &nbsp;&nbsp; 1. item4 </br>
> &nbsp;&nbsp; 2. item5

## `<input>`
* 다양한 입력 폼
* EX. 아이디, 비밀번호, 텍스트박스, 체크박스 등
```html
ID/PW
<input type="text" placeholder="type here">
<input type="password">

checkbox
<input type="checkbox">다음에 들어올 때 아이디 기억하기

radiobox
<input type="radio" name="option1"> 옵션A
<Input type="radio" name="option1"> 옵션B
```
</br>

# 시멘틱 태그 개념
```
시멘틱 태그(Semantic Tag)는 "의미 있는 태그"라는 뜻이다.
즉, HTML tag들 중에서도 의미가 있는 태그들을 말한다.
```
### 중요한 이유
1. 유지보수성 </br>
단순히 `<div>`, `<tag>`만으로 구조를 짜는 것보다 더 한 눈에 알아볼 수 있기 때문에, 다른 개발자들이 코드를 유지보수하기 편해진다.

2. SEO (Search engine optimization) </br>
시멘틱 태그를 잘 활용하면 특정 키워드로 검색했을 때, 나의 웹사이트가 검색창에 노출될 수 있다.

3. 웹 접근성 </br>
적절한 시멘틱 태그로 만들어진 웹사이트는 음성으로 읽어주는 스크린리더나 키보드만을 사용해도 문제없이 동작할 수 있다.
