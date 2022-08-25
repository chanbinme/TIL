# 목차
* [목차](#목차)
* [CSS](#css)
    + [기본 문법](#기본-문법)
* [기본적인 선택자](#기본적인-선택자selector)
    + [id](#id)
    + [class](#class)
    + [여러 개의 class를 하나의 엘리먼트에 적용하기](#여러-개의-class를-하나의-엘리먼트에-적용하기)
* [텍스트 꾸미기](#텍스트-꾸미기)
    + [색상](#색상)
    + [글꼴](#글꼴)
    + [크기](#크기)
    + [정렬](#정렬)
    + [기타 스타일링](#기타-스타일링)
* [박스 모델](#박스-모델)
    + [block vs. inline](#block-vs-inline)
    + [박스의 구성 요소](#박스의-구성-요소)
        + [border](#border-테두리)
        + [margin](#margin-바깥-여백)
        + [padding](#padding-안족-여백)
    + [박스 크기 측정 기준](#박스-크기-측정-기준)


# CSS
<pre>
CSS(Cascading Style Sheets)는 웹 페이지 스타일 및 레이아웃을 정의하는 스타일 시트 언어이다.
</pre>

## 기본 문법
* CSS는 선택자(selector)와 선언부(declaratives)로 구성된다.
* 선택자(selector)는 CSS를 적용하고자 하는 HTML 요소(element)를 가리킨다.
* 선언(declaration)은 속성명(property name)과 속성값(property value)를 사용하여 속성을 변경할 수 있다. 선언은 언제나 마지막에 세미콜론(;)으로 끝마친다. 
```css
셀렉터 {
    속성명 : 속성값;
}
```
```css
body {
    color : red;
}
```

# 기본적인 선택자(selector)
## `id`
* `id`는 하나의 문서에서 한 요소에만 사용해야 한다.
* `id`가 있는 요소를 선택할 때에는 `#`기호를 사용한다.
* 스타일링 하려는 요소에 `id`를 붙인 후 `id`로 요소를 선택해 스타일링한다.
```html
<h4 id="navigation-title">This is the navigation section.</h4>
```
```css
#navigation-title {
    color: red;
}
```

## `class`
* `class`는 다수의 요소에 사용할 수 있다.
* `class`가 있는 요소를 선택할 때에는 `.`기호를 사용한다.
* 스타일링 하려는 요소들에 `class`를 붙인 후 `class`로 요소를 선택해 스타일링한다.
* 작업을 진행하다보면 유일한 요소보다는 다수의 요소에 특정 속성 값을 부여해주어야 하는 경우가 많기 때문에 `id`보다 `class`가 자주 사용된다.
```html
<ul>
	<li class="menu-item">Home</li>
  <li class="menu-item">Mac</li>
	<li class="menu-item">iPhone</li>
	<li class="menu-item">iPad</li>
</ul>
```
```css
.menu-item {
  text-decoration: underline;
}
```

`id`|`class`
-|-
`#`으로 선택|`.`으로 선택
한 문서에 단 하나의 요소에만 적용|동일한 값을 갖는 요소가 많음
특정 요소에 이름을 붙이는 데 사용|스타일의 분류에 사용
## 여러 개의 `class`를 하나의 엘리먼트에 적용하기
* 여러 `class`를 하나의 요소에 적용하기 위해서는 띄어쓰기로 적용하려는 `class`들의 이름을 구분
* 요소를 만들거나, 요소에 스타일링을 적용할 때에 항상 이름과 목적이 일치하는지 확인
```html
<li class="menu-item selected">Home</li>
```
```css
.menu-item {
  text-decoration: underline;
}

.selected {
  font-weight: bold;
  color: #009999;
}
```

# 텍스트 꾸미기
## 색상
* 글자의 색상을 변경하는 속성은 color
* 속성에 삽입할 수 있는 값은 HEX(16진수 RGB; Red Green Blue가 표현된 값) 또는 주요 색상의 이름을 사용
```css
.box {
  color: #155724; /* 글자 색상 */
  background-color: #d4edda; /* 배경 색상 */
  border-color: #c3e6cb; /* 테두리 색상 */
}
```

## 글꼴
* 글꼴의 속성은 font-family
* 글꼴의 이름은 따옴표를 붙여서 사용
* 사용하려는 글꼴이 존재하지 않거나, 디바이스에 따라 지원하지 않는 경우를 대비하여 fallbac 글꼴을 추가할 수 있다.
* 웹 폰트 서비스 [Google Fonts](https://fonts.google.com/)
* [웹 폰트 기술](https://d2.naver.com/helloworld/4969726)로 개발자가 표현하고 싶은 글꼴을 사용자의 기기에 적용할 수 있다.
```css
.emphasize {
  font-family: "SF Pro KR", "MalgunGothic", "Verdana";
}
```

## 크기
* 글자의 크기를 변경하기 위한 속성은 font-size
```css
.title {
  font-size: 24px;
}
```
* 절대 단위 : `px`,`pt`등
  + 기기나 브라우저 사이즈등의 환경에 영향을 받지 않는 절대적인 크기로 정하는 경우 `px`(픽셀)을 사용
  + 글꼴의 크기를 고정하는 단위이기 때문에 사용자 접근성이 불리
  + 인쇄와 같이 화면의 사이즈가 정해진 경우 유리
* 상대 단위 : `%`, `em`, `rem`, `ch`, `vw`, `vh`등
  + 일반적인 경우 `rem`을 추천. 사용자가 설정한 기본 글꼴 크기를 따르므로, 접근성에 유리하다.

## 정렬
* 가로 정렬 `text-align` : 유효한 값으로 `left`, `right`, `center`, `justify`
* 세로 정렬 `vertical-align` : 이 속성은 부모 요소의 `display` 속성이 반드시 `table-call`이어야 한다.

## 기타 스타일링
* 굵기 : `font-weight`
* 밑줄, 가로줄 : `text-decoration`
* 자간 : `letter-spacing`
* 행간 : `line-height`

# 박스 모델
* 웹 페이지 내의 모든 콘텐츠는 고유의 영역을 가지고 있다. 그 영역을 박스(box)라고 한다.
* 일반적으로 하나의 콘텐츠로 묶이는 엘리먼트(요소)들이 하나의 박스가 된다.
* 박스는 항상 직사각형이고, 높이(height)와 넓이(width)를 가진다.
![박스 구조](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F19d6adaf-bc37-4a59-8652-4d7921efdce2%2FUntitled.png?table=block&id=8963aa8d-f2ea-49aa-b20b-7d52f22de13b&spaceId=0a158548-5249-4414-af12-5841e93de16e&width=1460&userId=24887374-568c-40b6-9f76-158b7eb504c1&cache=v2)

## block vs. inline
* block : 줄바꿈이 되는 박스. 대표적으로 `<div>`, `<p>`가 있다. [block 엘리먼트 목록](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements)
* inline : 옆으로 붙는 박스. 크기 지정을 할 수 없다. 대표적으로 `<span>`가 있다. [inline 엘리먼트 목록](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements)
* inline-block : 줄바꿈이 일어나지 않는 동시에 block의 특징을 가진다.

 .|block|inline-block|inline
 ---|---|---|---
 줄바꿈 여부|줄바꿈이 일어남|줄바꿈이 일어나지 않음|줄바꿈이 일어나지 않음
 기본적으로 갖는 너비(width)|100%|글자가 차지하는 만큼|글자가 차지하는 만큼
 width, height 사용 가능 여부|가능|가능|불가능


## 박스의 구성 요소
![박스의 구성 요소](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ab50a7b3-c8ac-4ef2-a409-b913499959df/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T125549Z&X-Amz-Expires=86400&X-Amz-Signature=1dd21a31da619039e1d98f4a3e32544f3ba023e5f8ed7e0476af6e368d726d1f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### border (테두리)
* 테두리는 심미적인 용도 외에도, 개발 과정에서 매우 의미있게 사용
* 각 영역이 차지하는 크기를 파악하기 위해, 레이아웃을 만들면서 그 크기를 시각적으로 확인할 수 있도록 만듦
```css
border: 1px solid red;

border: border-width border-style border-color;
```

### margin (바깥 여백)
```css
margin: 10px 20px 30px 40px;
margin: top right bottom left;

margin: 10px 20px;
margin: top bottom;

margin: 10px;
margin: all
```
* 음수 값을 지정할 수 있다. 여백에 음수 값을 지정하면 다른 요소와의 간격이 줄어든다. 극단적으로 적용하면, 화면(viewport)에서 아예 사라지게 하거나, 다른 엘리먼트와 겹치게 만들 수도 있다.
```css
margin-top: -2rem;
```

### padding (안족 여백)
* border를 기준으로 내부의 여백을 지정
```css
padding: 10px 20px 30px 40px;
padding: top right bottom left;

padding: 10px 20px;
padding: topbottom leftright;

padding: 10px;
padding: all
```

## 박스 크기 측정 기준
* 박스 크기를 측정하는 기준은 매우 중요하다.
* 여백을 고려하지 않고 박스의 크기를 디자인하는 경우를 조심해야 한다.
```css
#container {
  width: 300px;
  padding: 10px;
  background-color: yellow;
  border: 2px solid red;
}

#inner {
  width: 100%;
  height: 200px;
  border: 2px solid green;
  background-color: lightgreen;
  padding: 30px;
}
```
* `id`가 `container`인 박스의 `width` 속성을 300px로 지정했지만 실제 확인된 `width` 값은 324px이다.
```css
300px (콘텐츠 영역)
+ 10px (padding-left)
+ 10px (padding-right)
+ 2px (border-left)
+ 2px (border-right)
```
* `#inner`의 `width` 100%는 300px이 아니라, 364px이다.
```css
300px  (300px의 100%)
+ 30px (padding-left)
+ 30px (padding-right)
+ 2px (border-left)
+ 2px (border-right)
```
* 여백과 테두리 두께를 포함한 박스 계산법을 사용하면 레이아웃 디자인을 조금 더 쉽게 할 수 있다. `*`은 모든 요소를 선택하는 셀렉터이다.
* 모든 요소를 선택해 `box-sizeing` 속성을 추가하고, `border-box`라는 값을 추가한다.
* 이렇게 적용하면, 모든 박스에서 여백과 테두리를 포함한 크기로 계산된다.
```css
* {
  box-sizing: border-box;
}
```
* `content-box`는 박스의 크기를 측정하는 기본값이다. 그러나 대부분의 레이아웃 디자인에서 여백과 테두리를 포함하는 박스 크기 계산법인 `border-box`를 권장한다.

![content-box, border-box](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/570a77af-17c0-4a0b-8fe3-0a52a02de959/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220825%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220825T130559Z&X-Amz-Expires=86400&X-Amz-Signature=2756c96b77203422785a88a9e1c3c26584d5f522a6f96827f2ffbdf8ee94c1b9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)