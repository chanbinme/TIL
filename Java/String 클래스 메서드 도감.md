
<p align="center">
    <img src="https://static.wikia.nocookie.net/pokemon/images/3/34/1%EC%84%B8%EB%8C%80_%EB%8F%84%EA%B0%90.png/revision/latest?cb=20141004064302&path-prefix=ko" alt="포켓몬스터 도감" width="700px" />
</p>

# 목차
* [목차](#목차)
* [split( )](#split)
* [equals( )](#equals)
* [replace( )](#replace)
    + [String.replace( )](#1-string-replace기존-문자-바꿀-문자)
    + [String.replaceAll](#2-string-replaceall기존-문자-또는-정규식-바꿀-문자)
    + [String.replaceFirst](#3-string-replacefirst기존-문자-또는-정규식-바꿀-문자)
* [charAt( )](#charat)
* [compareTo( )](#compareto)
* [concat( )](#concat)
* [indexOf( )](#indexof)
* [toLowerCase( )와 toUpperCase( )](#tolowercase-와-touppercase)
* [trim( )](#trim)
* [String.format( )](#stringformat)

계속 추가할 예정.

# Java 클래스 메서드 도감

### split()
> 지정된 분리자(regex)를 기준으로 문자열을 자르고 문자열 배열에 담아 반환해주는 메서드

```java
String[] split(String regex)
String[] split(String regex, int limit)
```
분리자(regex)으로 문자열 패턴을 받고, 패턴과 동일한 문자열을 기준으로 잘라준다. limit은 문자열을 나눌 수 있는 최대 개수이다. 
#### 예제
```java
String str = "010-1234-5678-9101";
String[] result1 = str.split("-");
String[] result2 = str.split("-", 2);
String[] result3 = str.split("-", 3);


//결과
result1 = [010, 1234, 5678, 9101]
result2 = [010, 1234-5678-9101]
result3 = [010, 1234, 5678-9101]
```
---

### equals()
> 매개변수로 받은 문자열(obj)과 String 인스턴스의 문자열을 비교한다. obj가 문자열이 아니거나 내용이 다르면 false를 반환한다. 

```java
boolean equals(Object obj)
```
euals( )는 최상위 클래스인 Object에 포함되어 있기 때문에 모든 하위 클래스에서 재정의하여 사용할 수 있다. 

#### 예제
```java
Stirng c = new String("BHC");
boolean chicken1 = c.equals("BHC");
boolean chicken2 = c.equals("BBQ");

//결과
chicken1 = true
chicken2 = false
```
equals( )와  `==` 는 어떤 차이가 있을까?
가장 중요한 차이점은 `==` 는 주소값이 같은지 아닌지를 비교하고, equals( )는 내용이 같은지 아닌지를 비교하는 것이다. String은 객체이기때문에 equals( )를 사용해서 비교해야 한다.
```java
String c1 = new String("BHC");
String c2 = new String("BHC");

System.out.println(c1 == c2);        //false
System.out.println(c1.equals(c2));   //true
```

---
### replace()
> 바꾸고 싶은 문자를 입력 받아 문자열을 치환시켜주는 메서드

```java
String replace(String target, String replacement)
String replaceAll(String regex, String replacement)
String replaceFirs(String regex, String replacement)
```
#### 예제
#### 1. `String replace(기존 문자, 바꿀 문자)`
replace 메서드는 문자열에서 변경하고 싶은 문자열을 찾아 변환한다.
```java
String str = "010.1234.5678.9101";
String str1 = str.replace("0", "a");
String str2 = str.replace(".", "a");
System.out.println(str);

// 결과
a1a.1234.5678.91a1
010a1234a5678a9101
```

#### 2. `String replaceAll(기존 문자 또는 정규식, 바꿀 문자)`
replace와 replaceAll의 차이점은 replace는 인자를 문자열로 입력받지만 replaceAll은 인자를 문자열 또는 정규식으로 입력 받는다. 
그래서 특수문자를 입력 받은 경우 replace에서는 치환이 가능한 반면 replaceAll은 특수문자를 정규식으로 입력받아 치환이 어렵다.
```java
String str = "010.1234.5678.9101";
String str1 = str.replaceAll("0", "a");
String str2 = str.replaceAll(".", "a"); 
String str3 = str.replaceAll("[0-4]", "a");
        
System.out.println(str1);
System.out.println(str2);
System.out.println(str3);
        
// 결과
a1a.1234.5678.91a1
aaaaaaaaaaaaaaaaaa // 정규식에서는 .은 모든 문자를 나타낸다.
aaa.aaaa.5678.9aaa
```
대신 인식해주는 방법이 있다.
- 특수문자는 괄호 `[]`로 감싼다.
- 괄호, ^는 앞에 \\\를 붙인다.
```java
String str = "010.1234.5678{9101)";
// |를 사용하여 한 번에 여러개의 문자를 치환할 수 있다.
str = str.replaceAll("[.]", "");
str = str.replaceAll("\\{|\\)", "");

System.out.println(str);

// 결과
010123456789101
```


#### 3. `String replaceFirst(기존 문자 또는 정규식, 바꿀 문자)`
replaceFirst는 문자열에서 처음으로 찾은 문자만 치환한다.
```java
String str = "010 1234 5678 9101";
String str1 = str.replace(" ", "");
String str2 = str.replaceAll(" ", "");
String str3 = str.replaceFirst(" ", "");

System.out.println(str1);
System.out.println(str2);
System.out.println(str3);

// 결과
010123456789101
010123456789101
0101234 5678 9101
```

----
### `charAt()` 
> 문자열의 특정 인덱스에 해당하는 문자를 char타입으로 반환해주는 메서드

```java
char charAt(int index)
```
#### 예제
```java
String str = "BHC Chicken";
char c = str.charAt(0);
System.out.println(c);

// 결과
B
```

---
### `compareTo()`
> 두개의 문자열을 비교하여 int 값으로 반환해주는 메서드.

```java
int compareTo(NumberSubClass referenceName)
int compareTo(String anotherString)
int compareToIgnoreCase(String anotherString)
```
compareTo( )는 문자열 비교, 숫자 비교로 나눌 수 있다.
#### 숫자형 비교
- 기준 값과 비교 값이 동일한 값일 경우 : 0
- 기준 값이 비교 값보다 큰 경우 : 1
- 기준 값이 비교 값보다 작은 경우 : -1
```java
int a = 1;
int b = 2;
int c = 3;

System.out.println(a.compareTo(2));
System.out.println(b.compareTo(2));
System.out.println(c.compareTo(2));

// 결과
-1
0
1
```
#### 문자열 비교
- 기준 값과 비교 값이 동일한 값일 경우 : 0
- 기준 값과 비교 값이 다를 경우 : 첫 번째 문자부터 순서대로 비교해서 처음으로 비교 값이 다른 위치의 문자를 아스키값(ASCII) 기준으로 비교처리
- 대소문자를 구분하지 않고 비교하고 싶다면 compareToIgnoreCase( )를 사용한다.
```java
String str = new String("abcd");

System.out.println(str.compareTo("bcef"));
System.out.println(str.compareTo("abcd"));
System.out.println(str.compareTo("Abcd"));
System.out.println(str.compareToIgnoreCase("Abcd"));

// 결과
-1	// a = 97 / b = 98
0
32	// a = 97 / A = 65
0
```

---
### `concat()`
> 문자열 뒤에 인자로 받은 문자열을 추가한 새로운 문자열을 반환하는 메서드. concat은 concatenate(연결시키다)의 약자이다.

```java
String concat(String text)
```
#### 예제
```java
String str = new String("BHC");
System.out.println(str.concat("뿌링클");

// 결과
BHC뿌링클
```

---
### `indexOf()`
> 문자열에서 인자로 받은 문자나 문자열이 처음으로 등장하는 위치의 인덱스를 반환하는 메서드. 문자열에 전달 받은 문자나 문자열이 포함되어 있지 않으면 -1을 반환한다.

```java
int indexOf(String target)
```
#### 예제
```java
String str = new String("BHC 뿌링클 존맛");
System.out.println(str.indexOf("B"));
System.out.println(str.indexOf("BBQ"));
System.out.println(str.indexOf("존맛"));

// 결과
0
-1
8
```

---
### `toLowerCase()` `와` `toUpperCase()`
> toLowerCase( )는 해당 문자열의 모든 문자를 소문자로 변환 해주는 메서드이다.
toUpperCase( )는 해당 문자열의 모든 문자를 대문자로 변환해주는 메서드이다.

```java
String toLowerCase()
String toUpperCase()
```

#### 예제
```java
String str = new String("BHC");
String a = str.toLowerCase();
String b = a.toUpperCase();

System.out.println(a);
System.out.println(b);

// 결과
bhc
BHC
```

---
### `trim()`
> 해당 문자열의 맨 앞과 맨 뒤에 포함된 모든 공백 문자를 제거해주는 메서드. 중간에 있는 공백은 제거하지 않는다.

```java
String trim()
String trimStart()
String trimEnd()
```
#### 예제
```java
String str1 = "   BHC   ";
String str2 = "   B H C   ";

System.out.println(str1.trim());
System.out.println(str2.trim());

// 결과
BHC
B H C
```

---
### `String.format()`
> 문자열을 정의된 format대로 치환하여 반환하는 메서드

```java
static String format(String format, Object...args)
```
#### 지시자
지시자|설명
-|-
%b|boolean 타입으로 출력
%d|10진수(decimal) 타입으로 출력
%o|8진수(octal) 타입으로 출력
%x, %X|16진수(hexa-decimal) 타입으로 출력
%f|실수(floating-point) 타입으로 출력
%e, %E|지수(exponent) 표현식으로 출력
%c|문자(character) 타입으로 출력
%s|문자열(string) 타입으로 출력







