# 목차
* [목차](#목차)
* [생성자(Constructor](#생성자constructor)
    + [기본 생성자(Default Constructor)](#기본-생성자default-constructor)
    + [매개 변수가 있는 생성자](#매개-변수가-있는-생성자)
    + [this()](#this)
    + [this](#this-1)
* [내부 클래스(inner class)](#내부-클래스inner-class)


# 생성자(Constructor)

> 인스턴스 변수들을 초기화하는 데 사용되는 특수한 메서드. 인스턴스가 생성될 때 호출된다.
> 
- 생성자는 메서드와 두 가지 차이점이 있다.
    1. **생성자의 이름은 클래스의 이름과 같아야 한다.** 클래스 이름과 생성자 이름이 같지 않다면 생성자로서의 기능을 수행할 수 없다.
    2. **생성자는 리턴 타입이 존재하지 않는다.** 메서드에서 사용하는 void는 ‘리턴하지 않는다'는 의미를 가지고 있지만 생성자는 리턴 타입 자체가 존재하지 않는다.

```java
클래스명(매개변수) {  // 생성자 기본 구조
	...생략...
}
```

- 생성자도 오버로딩이 가능하다.

```java
public class ChickenSample {
	public static void main(String[] args) {
		Chicken chicken1 = new Chicken();
		Chicken chicken2 = new Chicken("BHC 뿌링클 존맛");
		Chicken chicken3 = new Chicken(7000, 20);

class Chicken {
	Chicken() {
		System.out.println("BBQ 생성자");
	}
	
	Chicken(String str) {
		System.out.println("BHC 생성자");
	}
	
	Chicken(int a, int b) {
		System.out.println("당당치킨 생성자);
	}
}

// 결과
BBQ 생성자
BHC 생성자
당당치킨 생성자
```

## 기본 생성자(Default Constructor)

- 모든 클래스는 반드시 하나 이상의 생성자가 존재해야 한다.
- 생성자를 따로 만들지 않으면 자바 컴파일러가 기본 생성자를 자동으로 추가해준다.
- 기본 생성자는 매개변수와 바디에 아무런 내용이 없다.
- 다른 생성자가 이미 있는 경우 기본 생성자가 아니라 추가되어어 있는 생성자를 기본으로 사용한다.

```java
클래스명(){}  // 기본 생성자

DefaultConst(){}  // EX.DefalutConst 클래스의 기본 생성자
```

## 매개 변수가 있는 생성자

- 매개변수를 통해 호출 시에 해당 값을 받아 인스턴스를 초기화하는 데 사용
- 고유한 특성을 가진 인스턴스를 계속 만들어야하는 경우 인스턴스마다 각기 다른 값을 가지고 초기화할 수 있어서 매우 유용하다.
- 매개변수가 있는 생성자를 사용하면 생성과 동시에 원하는 값으로 설정해줄 수 있어서 편리하다.

## this()

> this() 메서드는 자신이 속한 클래스에서 다른 생성자를 호출하는 경우에 사용한다.
> 
- `this()` 메서드를 사용하기 위해서는 두 가지의 문법 요소를 충족시켜야 한다.
    1. 반드시 생성자의 내부에서만 사용할 수 있다.
    2. 반드시 생성자의 첫 줄에 위치해야 한다.

```java
public class Test {
    public static void main(String[] args) {
        Example example = new Example();
        Example example2 = new Example(3);
	}
}

class Example {
	public Example() {
		System.out.println("기본 생성자");
	}
	
	public Example(int a) {
		this();
		System.out.println("매개변수가 있는 생성자");
	}
}

// 결과
기본생성자
기본생성자
매개변수가 있는 생성자
```

## this

> this는 인스턴스 자신을 가리키며, this를 통해서 인스턴스 자신의 변수에 접근할 수 있다.
> 
- 자바에서는 지역 변수명이 필드명과 동일하게 구성하기때문에 필드명과 지역변수를 구분하기 위한 용도로 사용된다.
- 모든 메서드에는 자신이 포함된 클래스의 객체를 가리키는 this라는 참조변수가 있는데, 컴파일러가 `this.` 를 자동으로 추가해준다.

```java
class Chicken {
    private String brandName;
    private String bestMenu;

    public Chicken(String brandName, String bestMenu) {
        this.bestMenu = bestMenu;  // 필드 변수 bestMenu에 지역 변수 bestMenu 선언
        this.brandName = brandName;
    }

    public String getBrandName() {
        return brandName;
    }

    public String getBestMenu () {
        return bestMenu;
    }
}

public class Main {
    public static void main(String[] args) {
        Chicken bbq = new Chicken("BHC", "뿌링클");
        System.out.println("나는 " + bbq.getBrandName() + "의 " + bbq.getBestMenu() + "을 가장 좋아합니다.");
    }
}

// 결과
나는 BHC의 뿌링클을 가장 좋아합니다.
```

# 내부 클래스(Inner Class)

> 클래스 내부에 선언된 클래스
> 
- 내부 클래스를 사용하면 외부 클래스의 멤버들에 쉽게 접근할 수 있고, 코드의 복잡성을 줄일 수 있다.
- 외부적으로 불필요한 데이터를 감출 수 있어 객체지향의 중요한 핵심 원칙인 캡슐화(encapsulation)를 달성하는 데 유용하다.
- 내부 클래스의 종류는 세 가지가 있다.

| 종류 | 선언 위치 | 사용 가능한 변수 |
| --- | --- | --- |
| 인스턴스 내부 클래스(instance inner class) | 외부 클래스의 멤버변수 선언 위치에 선언(멤버 내부 클래스) | 외부 인스턴스 변수, 외부 전역 변수 |
| 정적 내부 클래스(static inner class) | 외부 클래스의 멤버변수 선언 위치에 선언(멤버 내부 클래스) | 외부 전역 변수 |
| 지역 내부 클래스(local inner class) | 외부 클래스의 메서드나 초기화블럭 안에 선언 | 외부 인스턴스 변수, 외부 전역 변수 |
| 익명 내부 클래스(anonymous inner class) | 클래스의 선언과 객체의 생성을 동시에 하는 일회용 익명 클래스 | 외부 인스턴스 변수, 외부 전역 변수 |

```java
class Outer { // 외부 클래스
	
	class Inner {
		// 인스턴스 내부 클래스	
	}
	
	static class StaticInner {
		// 정적 내부 클래스
	}

	void run() {
		class LocalInner {
		// 지역 내부 클래스
		}
	}
}
```