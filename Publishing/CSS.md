# 목차
* [목차](#목차)
* [CSS](#css)
    + [기본 문법](#기본-문법)
* [기본적인 선택자](#기본적인-선택자selector)
    + [id](#id)
    + [class](#class)
    + [여러 개의 class를 하나의 엘리먼트에 적용하기](#여러-개의-class를-하나의-엘리먼트에-적용하기)
* [텍스트 꾸미기]
    + [색상]
    + [글꼴]
    + [크기]
    + [정렬]
    + [기타 스타일링]
* [박스 모델]    
    + [block vs. inline]
    + [박스의 구성 요소]
        + [border]
        + [margin]
        + [padding]
    + [박스 크기 측정 기준]

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
<!-- Markdown -->
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