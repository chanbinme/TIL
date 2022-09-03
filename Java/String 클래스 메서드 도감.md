
<p align="center">
    <img src="https://static.wikia.nocookie.net/pokemon/images/3/34/1%EC%84%B8%EB%8C%80_%EB%8F%84%EA%B0%90.png/revision/latest?cb=20141004064302&path-prefix=ko" alt="포켓몬스터 도감" width="700px" />
</p>

# 목차
* [목차](#목차)
* [split( )](#1-split)
* [equals( )](#2-equals)
* [replace( )](#replace)
    + [String.replace( )](#1-string-replace기존-문자-바꿀-문자)
    + [String.replaceAll](#2-string-replaceall기존-문자-또는-정규식-바꿀-문자)
    + [String.replaceFirst](#3-string-replacefirst기존-문자-또는-정규식-바꿀-문자)


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








