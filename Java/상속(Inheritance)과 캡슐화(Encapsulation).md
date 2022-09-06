# 목차
* [목차](#목차)
* [상속(Inheritance)](#상속inheritance)
    + [상속 방법](#상속-방법)
    + [포함(composite)](#포함composite)
    + [상속 관계와 포함 관계 판별 기준](#상속-관계와-포함-관계-판별-기준)
    + [메서드 오버라이딩(Method Overriding)](#메서드-오버라이딩method-overriding)
    + [super](#super)
    + [super()](#super-1)
    + [Object 클래스](#object-클래스)
* [캡슐화(Encapsulation)](#캡슐화encapsulation)
    + [패키지(package)](#패키지package)
    + [import문](#import문)
    + [접근 제어자(Access Modifier)](#접근-제어자access-modifier)
    + [getter와 setter 메서드](#getter와-setter-메서드)
        + [setter 메서드](#setter-메서드)
        + [getter 메서드](#getter-메서드)

# 상속(Inheritance)

> 기존 클래스를 재활용하여 새로운 클래스를 작성하는 자바의 문법 요소
> 
- 상속은 코드의 재사용성을 높이고 코드의 중복을 제거하여 프로그램의 생산성과 유지보수에 크게 기여한다. 또한 다형성을 구현할 수 있다는 점이 장점이다. (하나의 객체가 여러 모양으로 표현될 수 있다는 것을 다형성이라 부른다)
- 하위 클래스는 상위 클래스가 가진 모든 멤버(속성과 기능)를 상속 받는다. 이 두 클래스를 상속 관계에 있다고 한다.
- 하위 클래스의 멤버 개수는 언제나 상위 클래스의 멤버 개수보다 같거나 많다.
- 자바는 단일 상속(single inhertance)만을 지원한다. 인터페이스(interface)를 통해 다중 상속과 비슷한 효과를 낼 수 있다.

## 상속 방법

- 클래스명 다음에 `extends 상위클래스`를 사용해 클래스를 상속할 수 있다.
- 하위 클래스가 여러 개의 상위 클래스를 상속 받는 것은 불가능하지만 상위 클래스는 여러개의 하위 클래스에게 상속하는 것이 가능하다.

```java
class 하위클래스 extends 상위클래스 { ... }
class 하위클래스 extends 상위클래스1, 상위클래스2 { ... }  // 불가능
```

```java
class 하위클래스1 extends 상위클래스 { ... }
class 하위클래스2 extneds 상위클래스 { ... }
```

```java
class 상위클래스 extends 조상클래스 { 내용 }
class 하위클래스 extends 상위클래스 { 내용 } // 가능
```

```java
class Person {
    String name;
    int age;

    void walk(){
        System.out.println("걷는다.");
    };
    void eat(){
        System.out.println("먹는다.");
    };
}

class Ironman extends Person {
    String heroName;

    void protect() {
        System.out.println("지구를 지킨다.");
    }
}

public class PersonTest {
    public static void main(String[] args) {
        Person p = new Person();
        p.name = "김찬빈";
        p.age = 29;
        p.walk();
        p.eat();
        System.out.println(p.name);
        System.out.println();
        
        Ironman im = new Ironman();
        im.name = "토니 스타크";
        im.heroName = "아이언맨";
        im.age = 53;
        im.eat();
        im.walk();
        im.protect();
        System.out.println(im.name + ", " + im.heroName);
    }
}

// 결과
걷는다.
먹는다.
김찬빈

먹는다.
걷는다.
지구를 지킨다.
토니 스타크, 아이언맨
```

## 포함(composite)

> 상속처럼 클래스를 재사용할 수 있는 방법으로, 클래스의 멤버로 다른 클래스 타입의 참조변수를 선언하는 것
> 

```java
public class Avengers {
    int id;
    String name;
    Address address;

    public Avengers(int id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }

    void showInfo() {
        System.out.println(id + " " + name);
        System.out.println(address.city+ " " + address.country);
    }

    public static void main(String[] args) {
        Address address1 = new Address("맨해튼", "미국");
        Address address2 = new Address("퀸스 포리스트힐스", "미국");

        Avengers e = new Avengers(1, "아이언맨", address1);
        Avengers e2 = new Avengers(2, "스파이더맨", address2);

        e.showInfo();
        e2.showInfo();
    }
}

class Address {
    String city, country;

    public Address(String city, String country) {
        this.city = city;
        this.country = country;
    }
}

// 결과
1 아이언맨
맨해튼 미국
2 스파이더맨
퀸스 포리스트힐스 미국
```

## 상속 관계와 포함 관계 판별 기준

- ‘IS-A 관계(Is a relationship; inheritance)’라는 용어가 있다. IS-A 관계란 일반적인 개념과 구체적인 개념의 관계이다. 예를 들어 ‘사람은 포유류이다’와 같은 관계다.
- 상속은 IS-A 관계에서 사용하는 것이 효율적이다. 일반 클래스를 점차 구체화하는 상황에서 상속을 사용한다.
- 하위 클래스가 상위 클래스에 종속되기 때문에 이질적인 클래스 간에는 상속을 사용하지 않는 것이 좋다.
- 반대로 ‘HAS-A 관계(has a relationship; association)’라는 용어가 있다. HAS-A 관계란 한 클래스가 다른 클래스를 소유한 관계이다. 이럴 경우에는 상속을 사용하지 않는 것이 좋다.

## 메서드 오버라이딩(Method Overriding)

> 상위 클래스로부터 상속받은 메서드와 동일한 이름의 메서드를 재정의하는 것
> 
- 메서드 오버라이딩을 사용하기 위해서는 세 가지 조건을 만족시켜야 한다.
    - 메서드의 선언부(메서드 이름, 매개변수, 반환타입)이 상위 클래스와 완전히 동일해야한다.
    - 접근 제어자의 범위가 상위 클래스의 메서드보다 같거나 넓어야 한다.
    - 예외는 상위 클래스의 메서드보다 많이 선언할 수 없다.

```java
class Chicken {
    void eat() {
        System.out.println("치킨을 먹는다.");
    }
}

public class BHC extends Chicken { // Chicken 클래스 상속
    void eat() {
        System.out.println("뿌링클을 먹는다."); // 메서드 오버라이딩
    }

    public static void main(String[] args) {
        BHC ch = new BHC();
        ch.eat();
    }
}

// 결과
"뿌링클을 먹는다."
```

## super

> super는 상위 클래스의 인스턴스를 가리키며, super를 통해서 상위 클래스의 멤버 변수에 접근할 수 있다.
> 
- spuer는 자신의 인스턴스 변수와 상위 클래스의 인스턴스 변수를 구분하기 위한 방법으로 사용된다.
- super를 사용하면 상위 클래스의 인서턴스 멤버 값을 참고할 수 있다.
- 상위 클래스의 멤버와 자신의 멤버를 구별하는 데 사용된다는 점을 제외하면 this와 동일하다.

```java
public class Super {
    public static void main(String[] args) {
        Lower l = new Lower();
        l.callNum();
    }
}

class Upper {
    int count = 20; // super.count
}

class Lower extends Upper {
    int count = 15; // this.count

    void callNum() {
        System.out.println("count = " + count);
        System.out.println("this.count = " + this.count);
        System.out.println("super.count = " + super.count);
    }
}

// 출력값
count = 15
count = 15
count = 20
```

## super( )

> super( )는 상위 클래스의 생성자를 호출한다.
> 
- this( )와 마찬가지로 생성자 안에서만 사용 가능하고, 반드시 첫 줄에 와야 한다.
- 모든 생성자의 첫 줄에는 반드시 this( ) 또는 super( )가 선언되어야 한다.
- 만약 super( )가 없는 경우에는 컴파일러가 생성자의 첫 줄에 자동으로 super( )를 삽입한다. 이때 상위 클래스에 기본 생성자가 없으면 에러가 발생한다.

```java
public class Test {
    public static void main(String[] args) {
        Student s = new Student();
    }
}

class Human {
    Human() {
        System.out.println("휴먼 클래스 생성자");
    }
}

class Student extends Human { // Human 클래스로부터 상속
    Student() {    
        super(); // Human 클래스의 생성자 호출
        System.out.println("학생 클래스 생성자");
    }
}

// 출력값
휴먼 클래스 생성자
학생 클래스 생성자
```

## Object 클래스

> 자바의 클래스 상속계층도에서 최상위에 위치한 상위클래스이다. 따라서 자바의 모든 클래스는 Object 클래스로부터 확장된다는 명제는 항상 참이다.
> 
- 아무런 상속을 받지 않는 클래스는 컴파일러가 자동적으로 `extends Object` 를 추가하여 Object 클래스를 상속받도록 한다.
- Object 클래스의 멤버들도 자동으로 상속받아 사용할 수 있다.

```java
class Chicken {  // 컴파일러가 "extends Object" 자동 추가
}

class BHC extends Chicken { ... }
```

- 자주 쓰이는 Object 클래스의 메서드

| 메서드명 | 반환 타입 | 주요 내용 |
| --- | --- | --- |
| toString() | String | 객체 정보를 문자열로 출력 |
| equals(Object obj) | boolean | 등가 비교 연산(==)과 동일하게 스택 메모리값을 비교 |
| hashCode() | int | 객체의 위치정보 관련. Hashtable 또는 HashMap에서 동일 객체여부 판단 |
| wait() | void | 현재 쓰레드 일시정지 |
| notify() | void | 일시정지 중인 쓰레드 재동작 |

# 캡슐화(Encapsulation)

> 특정 객체 안에 관련된 속성과 기능을 하나의 캡슐(capsule)로 만들어 데이터를 외부로부터 보호하는 것
> 
- 캡슐화를 해야하는 이유는 크게 두 가지 목적이 있다. 한마디로 정보 은닉(data hiding)에 용이하다.
    - 데이터 보호의 목적
    - 내부적으로만 사용되는 데이터에 대한 불필요한 외부 노출을 방지
- 외부로부터 객체의 속성과 기능이 함부로 변경되지 못하게 막고, 데이터가 변경되더라도 다른 객체에 영향을 주지 않기때문에 독립성을 확보할 수 있다.
- 유지보수와 코드 확장 시에도 오류의 범위를 최소화할 수 있다.

## 패키지(package)

> 패키지란 특정한 목적을 공유하는 클래스와 인터페이스의 묶음을 의미한다.
> 
- 패키지는 클래스들을 그룹 단위로 묶어 효과적으로 관리하기 위한 목적을 가지고 있다.
- 자바에서 패키지란 하나의 디렉토리(directory)이고, 하나의 패키지에 속한 클래스나 인터페이스 파일은 모두 해당 패키지에 속해있다.
- 패키지로 클래스르 묶어 놓으면 클래스의 충돌을 방지해주는 기능이 있다. 예를 들어 같은 이름의 클래스더라도 패키지가 다르면 충돌이 발생하지 않는다.
- 디렉토리는 하나의 계층구조를 가지고 있는데, 계층 구조 간 구분은 점(.)으로 표현한다.
- 패키지가 있는 경우 소스 코드의 첫 번째 줄에 반드시 `package 패키지` 이 표시되야 한다.

```java
package chicken; 

public class BBQ {

}
```

- 대표적인 패키지
    - `java.lang` : 자바의 기본 클래스들을 모아 놓은 패키지
    - `java.util` : 자바의 확장 클래스를 묶어 놓은 패키지
    - `java.io` , `java.nio` : 자바의 입출력과 관련된 클래스를 묶어 놓은 패키지

## import문

> import문은 다른 패키지 내의 클래스를 사용하기 위해 사용한다.
> 

```java
import 패키지명.클래스명; 또는 import 패키지명.*;
```

- 소스 코드에서는 패키지 구문과 클래스문 사이에 작성한다.

```java
package practicepack.test2; // import문을 사용하는 경우

import practicepack.test.ExampleImp // import문 작성

public class PackageImp {
		public static void main(String[] args) {
			ExampleImp x = new ExampleImp(); // 이제 패키지명을 생략 가능
		}
}
```

## 접근 제어자(Access Modifier)

- 자바에서 제어자는 크게 두 가지로 구분한다.
    - 접근 제어자 : public, protected, defalut, private
    - 기타 제어자 : static, final, abstract, native, transient, synchronized 등
- 하나의 대상에 여러 제어자를 사용할 수 있다. **단, 접근 제어자는 단 한 번만 사용할 수 있다.**

### 접근 제어자

- 접근 제어자를 통해 외부로부터 데이터를 보호하고, 불필요하게 데이터가 노출되는 것을 방지할 수 있다.(데이터 보호와 은닉을 위한 효과적인 방법)
- 접근 제한 범위에 따라 표현하면 아래 순으로 정리할 수 있다.

```java
**public(접근 제한 없음) > protected(동일 패키지 + 하위클래스) > default(동일 패키지) > private(동일 클래스)** 
```

| 접근 제어자 | 동일 클래스 | 동일 패키지 | 다른 패키지의 하위 클래스 | 패키지 외 |
| --- | --- | --- | --- | --- |
| private | O | X | X | X |
| default | O | O | X | X |
| protected | O | O | O | X |
| public | O | O | O | O |

## getter와 setter 메서드

> 객체의 변수의 데이터 값을 추가하거나 수정할 수 있는 메서드
> 
- 데이터를 효과적으로 보호하면서도 의도하는 값으로 값을 변경하여 캡슐화를 보다 효과적으로 달성할 수 있다.

### setter 메서드

- 객체 외부에서 메서드에 접근하여 조건에 맞을 경우 데이터 값을 변경 가능하게 해준다.
- 메서드명 앞에 `set-` 을 붙여 사용한다.

```java
private  int price;

void setPrice(int num) {
	this.price = price;
}
```

### getter 메서드

- 설정한 객체의 데이터 값을 변수에 담아 객체 외부에 출력해준다.
- 메서드명 앞에 `get-` 을 붙여 사용한다.

```java
//Getter 메소드 예시
private int price;

int getPrice() {
	return price;
}
```