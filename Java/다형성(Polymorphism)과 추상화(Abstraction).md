# 목차
* [목차](#목차)
* [다형성(Polymorphism)](#다형성polymorphism)
    + [참조변수의 형 변환](#참조변수의-형-변환)
    + [instanceof 연산자](#instanceof-연산자)
    + [예제](#예제)
* [추상화(Abstraction)](#추상화abstraction)
    + [abstract 제어자](#absract-제어자)
    + [추상 클래스](#추상-클래스)
    + [final](#final)
    + [인터페이스](#인터페이스)
        + [인터페이스의 기본 구조](#인터페이스의-기본-구조)
        + [인터페이스의 구현](#인터페이스의-구현)
        + [인터페이스의 다중 구현](#인터페이스의-다중-구현)
        + [예제](#예제)


# 다형성(Polymorphism)

> 상위 클래스 타입이 참조변수를 통해서 하위 클래스의 객체를 참조할 수 있도록 허용한 것
> 
- 다형성을 잘 활용하면 중복되는 코드를 줄이고 보다 편리하게 코드를 작성할 수 있다.
- 하위 클래스는 상위 클래스를 참조변수의 타입으로 지정했기 때문에 자연스럽게 참조변수가 사용할 수 있는 멤버의 개수는 상위 클래스의 멤버의 수가 된다.

```java
public class FriendTest {

    public static void main(String[] args) {
        Friend friend = new Friend(); // 객체 타입과 참조변수 타입의 일치 -> 가능
        BoyFriend boyfriend = new BoyFriend();
        Friend girlfriend = new GirlFriend(); // 객체 타입과 참조변수 타입의 불일치 -> 가능
				GirlFriend friend1 = new Friend(); //error // 하위클래스 타입으로 상위클래스 객체 참조 -> 불가능

        friend.friendInfo();
        boyfriend.friendInfo();
        girlfriend.friendInfo();
    }
}
```

- 메서드 오버로딩과 메서드 오버라이딩 둘 다 같은 이름의 메서드를 재사용 또는 덮어쓰기로 다르게 사용한다는 점을 봤을 때 다형성의 의미를 갖고 있다고 볼 수 있다.

## 참조변수의 형 변환

- 참조변수의 형 변환은 사용할 수 있는 멤버의 개수를 조절하는 것을 의미한다.
- 형 변환을 위해서는 세 가지 조건을 충족해야 한다.
    - 서로 상속 관계에 있어야 한다.
    - 하위 클래스 타입에서 상위 클래스 타입으로 형 변환(업캐스팅)은 형 변환 연산자(괄호)를 생략할 수 있다.
    - 상위 클래스 타입에서 하위 클래스 타입으로 형 변환(다운 캐스팅)은 형 변환 연산자(괄호)를 반드시 명시해야 한다.

## instanceof 연산자

> 참조변수가 참조하는 인스턴스의 실제 타입을 체크하는 문법 요소
> 

```java
 참조_변수 instanceof 타입
```

- boolean타입으로 연산 결과는 true, false로 나오며 연산결과가 true이면, 해당 타입으로 형 변환이 가능하다.

```java
public class InstanceOfExample {
    public static void main(String[] args) {
        Animal animal = new Animal();
        System.out.println(animal instanceof Object); //true
        System.out.println(animal instanceof Animal); //true
        System.out.println(animal instanceof Bat); //false

        Animal cat = new Cat();
        System.out.println(cat instanceof Object); //true
        System.out.println(cat instanceof Animal); //true
        System.out.println(cat instanceof Cat); //true
        System.out.println(cat instanceof Bat); //false
    }
}

class Animal {};
class Bat extends Animal{};
class Cat extends Animal{};
```

## 예제

```java
public class PolymorphismEx {
  public static void main(String[] args) {
    Customer customer = new Customer();
    customer.buyCoffee(new Americano());
    customer.buyCoffee(new CaffeLatte());

    System.out.println("현재 잔액은 " + customer.money + "원 입니다.");
  }
}

class Coffee {
  int price;

  public Coffee(int price) {
    this.price = price;
  }
}

class Americano extends Coffee {
  public Americano() {
    super(4000); // 상위 클래스 Coffee의 생성자를 호출
  }

  public String toString() {return "아메리카노";}; //Object클래스 toString()메서드 오버라이딩
};

class CaffeLatte extends Coffee {
  public CaffeLatte() {
    super(5000);
  }

  public String toString() {return "카페라떼";};
};

class Customer {
  int money = 50000;

  void buyCoffee(Coffee coffee) {
    if (money < coffee.price) { // 물건 가격보다 돈이 없는 경우
      System.out.println("잔액이 부족합니다.");
      return;
    }
    money = money - coffee.price; // 가진 돈 - 커피 가격
    System.out.println(coffee + "를 구입했습니다.");
  }
}

// 결과
아메리카노를 구입했습니다.
카페라떼를 구입했습니다.
현재 잔액은 41000원 입니다.
```

- 다형성이 가지는 특성에 따라 매개변수로 각각의 개별적인 커피의 타입이 아니라 상위클래스인 `Coffee`의 타입을 매개변수로 전달받으면, 그 하위클래스 타입의 참조변수는 매개변수로 전달될 수 있고 이에 따라 매번 다른 참조변수를 매개변수로 전달해줘야하는 번거로움을 훨씬 줄일 수 있다.

# 추상화(Abstraction)

> 기존 클래스들의 공통적인 요소들(속성과 기능)을 뽑아서 상위 클래스를 만들어 내는 것
> 
- 코드의 중복을 줄일 수 있다.
- 보다 효과적으로 클래스 간의 관계를 설정할 수 있다.
- 코드의 유지/보수가 용이해진다.
- 자바에서는 주로 추상 클래스와 인터페이스를 사용해서 추상화를 구현한다.

## absract 제어자

- `absract`는 클래스와 메서드를 형용하는 키워드로 사용된다.
- 메서드 앞에 붙은 경우 ‘추상 메서드(absract method)`, 클래스 앞에 붙을 경우 ‘추상 클래스(abstract class)’라고 부른다.
    - 추상 메서드 : 메서드의 시그니처만 있고 바디가 없는 메서드
    - 추상 클래스 : 추상 메서드를 1개 이상 포함하는 클래스
- 클래스에 추상 메서드가 하나라도 포함되어있는 경우 해당 클래스는 자동으로 추상 클래스가 된다.
- 추상 메서드와 추상 클래스는 충분히 구체화되지 않은 ‘미완성 메서드', ‘미완성 클래스'를 의미한다.

```java
abstract class AbstractExample { // 추상 메서드가 최소 하나 이상 포함돼있는 추상 클래스
	abstract void start(); // 메서드 바디가 없는 추상메서드
}
```

- 추상 클래스는 인스턴스 생성이 불가하다.

```java
AbstractExample abstractExample = new AbstractExample(); // 에러발생
```

## 추상 클래스

> 메서드 시그니처만 존재하고 바디가 선언되어있지 않은 추상 메서드를 포함하는 ‘미완성 설계도’
> 
- 추상 클래스는 상속 관계에 있어 새로운 클래스를 작성하는데 매우 유용하다.
- 상속을 받는 하위 클래스에서 오버라이딩을 통해 **각각 상황에 맞는 메서드 구현이 가능**하다는 장점이 있다.
- 설계하는 상황이 변하더라도 보다 **유연하게 대응할 수 있다.**
- 만약 여러 사람이 함께 개발하는 경우 공통적으로 사용되는 속성과 기능이 각각 다르게 정의되는 경우를 방지할 수 있다.
- 상속계층도의 상층부에 위치할수록 추상화의 정도가 높고 그 아래로 내려갈수록 구체화된다.
- 상층부에 가까울수록 더 공통적인 속성과 기능들이 정의되어 있다고 볼 수 있다.

## final

- 필드, 지역 변수, 클래스 앞에 위치할 수 있으며 공통적으로 변경이 불가능하고 확장할 수 없다는 특징이 있다.

| 위치 | final 제어자가 추가되었을 때의 의미 |
| --- | --- |
| 클래스 | 변경 또는 확장 불가능한 클래스, 상속 불가 |
| 메서드 | 오버라이딩 불가 |
| 변수 | 값 변경이 불가한 상수 |

```java
final class FinalEx { // 확장/상속 불가능한 클래스
	final int x = 1; // 변경되지 않는 상수

	final int getNum() { // 오버라이딩 불가한 메서드
		final int localVar = x; // 상수
		return x;
	}
}
```

## 인터페이스

> 추상 메서드의 집합
> 
- 추상 메서드와 상수만을 멤버로 가질 수 있다. (자바8 이후로 default/static 메서드를 포함 시킬 수 있게 되었다.)
- 추상 클래스에 비해 추상화 정도가 높다. 추상 클래스가 ‘미완성 설계도'라면 인터페이스는 ‘밑그림'이라고 볼 수 있다.
- 인터페이스의 장점
    - 다중적 구현이 가능하다.
    - 선언과 구현을 분리시켜 개발시간을 단축한다.
    - 독립적인 프로그래밍을 통해 한 클래스의 변경이 다른 클래스에 미치는 영향을 최소화해준다.
    - 코드 변경의 번거로움을 최소화하고 손쉽게 해당 기능을 사용할 수 있게 해준다.

### 인터페이스의 기본 구조

- 기존 클래스를 작성하는 것과 유사하지만, 인터페이스는 `class` 대신 `interface` 를 사용한다.
- 내부의 모든 필드는 `public static final`로 정의한다.
- `static` 과 `default` 메서드 이외의 모든 메서는 `public abstract` 로 정의한다.
- 다만, 모든 인터페이스의 필드와 메서드에는 위의 요소가 내포되어 있기 때문에 명시하지 않아도 생략할 수 있다.

```java
public interface InterfaceEx {
    public static final int rock =  1; // 인터페이스 인스턴스 변수 정의
    final int scissors = 2; // public static 생략
    static int paper = 3; // public & final 생략

    public abstract String getPlayingNum();
		void call() //public abstract 생략 
}
```

### 인터페이스의 구현

- 인터페이스는 `extends` 대신 `implements` 를 사용한다. implements는 ‘구현하다'라는 의미를 갖고 있다.

```java
class 클래스명 implements 인터페이스명 {
			...  // 인터페이스에 정의된 모든 추상 메서드 구현
}
```

- 인터페이스를 구현한 클래스는 반드시 **해당 인터페이스에 정의된 모든 추상메서드를 오버라디이하여 바디를 완성해야한다.**

### 인터페이스의 다중 구현

- 하나의 클래스가 여러 개의 인터페이스를 구현할 수 있다.
- 클래스에서 다중 상속이 불가능했던 이유는 부모 클래스에 동일한 이름의 필드 또는 메서드가 존재하는 경우 충돌이 발생하기 때문이다.
- 인터페이스는 미완성된 멤버를 가지고 있기 때문에 충돌이 발생할 여지가 없고, 다라서 안전하게 다중 구현이 가능하다.
- 인터페이스는 인터페이스로부터만 상속이 가능하고, 클래스와 달리 Object 클래스와 같은 최고 조상이 존재하지 않는다.

```java
class ExampleClass implements Interface1, Interface2, Interface3 { 
				...
}
```

### 예제

다음 시나리오로 인터페이스를 사용한 코드를 작성해보자

```java
카페를 운영하는 사람이 있습니다. 
단골손님들은 매일 마시는 음료가 정해져 있습니다.
단골손님A는 항상 아이스 아메리카노를 주문합니다. 
단골손님B는 매일 아침 딸기라떼를 구매합니다.
```

```java
interface Customer {   // Customer 인터페이스 생성
  String getOrder();
}

class CafeCustomerA implements Customer {   // Customer 인터페이스 구현
  public String getOrder(){
		return "a glass of iced americano";
	}
}

class CafeCustomerB implements Customer {   // Customer 인터페이스 구현
  public String getOrder(){
		return "a glass of strawberry latte";
	}
}

class CafeOwner {   // Customer 인터페이스를 매개변수로 사용
  public void giveItem(Customer customer) {
    System.out.println("Item : " + customer.getOrder());
  }
}

public class OrderExample {
    public static void main(String[] args) throws Exception {
        CafeOwner cafeowner = new CafeOwner();
        Customer cafeCustomerA = new CafeCustomerA();
        Customer cafeCustomerB = new CafeCustomerB();

        cafeowner.giveItem(cafeCustomerA);
        cafeowner.giveItem(cafeCustomerB);
    }
}

// 결과
Item : a glass of iced americano
Item : a glass of strawberry latte
```