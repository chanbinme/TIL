# 목차
* [목차](#목차)
* [열거형(Enum)](#열거형enum)
    + [enum에서 사용할 수 있는 메서드](#enum에서-사용할-수-있는-메서드)
* [제네릭(Generic)](#제네릭generic)
    + [제네릭의 필요성](#제네릭의-필요성)
    + [제네릭 사용시 주의할 점](#제네릭-사용시-주의할-점)
    + [제한된 제네릭 클래스](#제한된-제네릭-클래스)
    + [제네릭 메서드](#제네릭-메서드)
    + [와일드 카드](#와일드-카드)

# 열거형(Enum)

> 서로 연관된 상수들의 집합을 의미
> 
- 상수란 변하지 않는 값을 의미하며 `final` 키워드를 사용하여 선언
- 여러 상수들을 보다 편리하게 선언하고 관리할 수 있다.
- 상수명의 중복을 피하고, 타입에 대한 안전성을 보장한다.
- 간결하고 가독성이 좋은 코드를 작성할 수 있다.

```java
enum 열거형이름 { 상수명1, 상수명2, 상수명3, ... }
```

- 각각의 상수들에는 자동적으로 0부터 시작하는 정수값이 할당되어 각각 상수들을 가리키게 된다.

```java
enum Seasons {
SPRING, // 정수값 0 할당
SUMMER, // 정수값 1 할당
FALL, // 정수값 2 할당
WINTER  // 정수값 3 할당
```

- 사용자 정의 타입은 switch문에서 활용할 수 없지만 enum으로 정의한 상수는 switch문에서도 사용 가능하다.

```java
class Seasons {
    public static final Seasons SPRING = new Seasons();
    public static final Seasons SUMMER = new Seasons();
    public static final Seasons FALL   = new Seasons();
    public static final Seasons WINTER = new Seasons();
}

public class Main {
    public static void main(String[] args) {
        Seasons seasons = Seasons.SPRING;
        switch (seasons) {
            case Seasons.SPRING:
                System.out.println("봄");
                break;
            case Seasons.SUMMER:
                System.out.println("여름");
                break;
            case Seasons.FALL:
                System.out.println("가을");
                break;
            case Seasons.WINTER:
                System.out.println("겨울");
                break;
        }
    }
}

// 결과
에러 발생
```

```java
enum Seasons {SPRING, SUMMER, FALL, WINTER}

public class Main {
    public static void main(String[] args) {
        Seasons seasons = Seasons.SPRING;
        switch (seasons) {
            case SPRING:
                System.out.println("봄");
                break;
            case SUMMER:
                System.out.println("여름");
                break;
            case FALL:
                System.out.println("가을");
                break;
            case WINTER:
                System.out.println("겨울");
                break;
        }
    }
}

// 결과 
봄
```

## enum에서 사용할 수 있는 메서드

| 리턴 타입 | 메소드(매개변수) | 설명 |
| --- | --- | --- |
| String | name() | 열거 객체가 가지고 있는 문자열을 리턴하며, 리턴되는 문자열은 열거타입을 정의할 때 사용한 상수 이름과 동일합니다. |
| int | ordinal() | 열거 객체의 순번(0부터 시작)을 리턴합니다. |
| int | compareTo(비교값) | 주어진 매개값과 비교해서 순번 차이를 리턴합니다. |
| 열거 타입 | valueOf(String name) | 주어진 문자열의 열거 객체를 리턴합니다. |
| 열거 배열 | values() | 모든 열거 객체들을 배열로 리턴합니다. |

# 제네릭(Generic)

> 작성한 클래스 또는 메서드의 코드가 특정 데이터 타입에 얽매이지 않게 해두는 것
> 
- 타입 매개변수  `T` 를 임의의 타입으로 사용한다. `<T>` 와 같이 클래스 이름 옆에 작성해주면 클래스 몸체에 사용할 타입 매개변수를 선언할 수 있다.

```java
class Chicken<T> {
	private T chicken;
}

// 타입 매개변수를 여러 개 사용해야 할 때
class Chicken<K, V> { ... }
```

- 타입 매개변수로 `T` (Type), `K` (Key), `V` (Value), `E` (Element), `N` (Number), `R` (Result) 가 자주 사용된다.

## 제네릭의 필요성

- 클래스나 메서드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있다.
- 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있다.
- 제네릭을 사용하면 하나의 클래스만으로 모든 타입의 데이터를 저장할 수 있는 인스턴스를 만들 수 있다.

```java
class Basket<T> {
    private T item;

    public Basket(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }
}
```

```java
Basket<String>  basket1 = new Basket<>("Hello");
Basket<Integer> basket2 = new Basket<>(10);
Basket<Double>  basket3 = new Basket<>(3.14);
```

## 제네릭 사용시 주의할 점

- 클래스 변수에는 타입 매개변수를 사용할 수 없다. 만약, 클래스 변수에 타입 매개변수를 사용하게 되면 클래스 변수의 타입이 인스턴스 별로 달라지게 된다.

```java
class Chicken<T> {
	private T chicken1;
	static T chicken2; // X
```

- 타입 매개변수에 치환될 타입으로 기본 타입을 지정할 수 없다.(`int`, `double` 등)
- 기본 타입을 지정해야 할 때에는 래퍼 클래스(Wrapper Class)를 활용한다.(`Integer`, `Double` 등)

```java
Basket<int> basket2 = new Basket<>(10);  // X
Basket<double>  basket2 = new Basket<>(3.14);  // X

Basket<String>  basket1 = new Basket<>("Hello");
Basket<Integer> basket2 = new Basket<>(10);
Basket<Double>  basket2 = new Basket<>(3.14);
```

## 제한된 제네릭 클래스

- 기존에는 제네릭 클래스는 타입을 지정하는 데 제한이 없다.
- `extends` 를 사용하여 특정 클래스를 상속받은 클래스 또는 특정 인터페이스를 구현한 클래스만 타이으로 지정할 수 있도록 제한 할 수 있다.

```java
interface Plant { ... }
class Flower implements Plant { ... }
class Rose extends Flower implements Plant { ... }
class RosePasta { ... }

class Basket<T extends Plant> {
    private T item;
	
		...
}

public static void main(String[] args) {

		// 인스턴스화 
		Basket<Flower> flowerBasket = new Basket<>();
		Basket<Rose> roseBasket = new Basket<>();
		Basket<RosePasta> rosePastaBasket = new Basket<>(); // 에러
}
```

- 특정 클래스를 상속받으면서 동시에 특정 인터페이스를 구현한 클래스만 타입으로 지정할 수 있도록 제한하려면 `&` 을 사용해야 한다.
- 클래스를 인터페이스보다 앞에 위치 시켜야 한다.

```java
class 클래스이름<T extends 상속받은 클래스 & 구현한 클래스> { ... }
```

```java
interface Plant { ... }
class Flower implements Plant { ... }
class Rose extends Flower implements Plant { ... }
class RosePasta implements Plant { ... }

class Basket<T extends Flower & Plant> { // (1)
    private T item;
	
		...
}

public static void main(String[] args) {

		// 인스턴스화 
		Basket<Flower> flowerBasket = new Basket<>();
		Basket<Rose> roseBasket = new Basket<>();
		Basket<RosePasta> rosePastaBasket = new Basket<>();  // 에러
}
```

## 제네릭 메서드

> 클래스 내부의 특정 메서드만 제네릭으로 선언한 메서드
> 
- 제네릭 메서드 내에서만 선언한 타입 매개변수를 사용할 수 있다.

```java
class Chicken {
	public <T> void add(T element) {
			...
	}
}
```

- 제네릭 메서드의 타입 매개변수는 제네릭 클래스의 타입 매개변수와 별개이다.
- 이유는 타입이 지정되는 시점이 서로 다르기 때문이다.
    - 제네릭 클래스 : 클래스가 인스턴스화 될 때 타입 지정
    - 제네릭 메서드 : 메서드가 호출될 때 타입 지정

```java
class Chicken<T> {   // 제네릭 클래스의 타입 매개변수 T와 
		public <T> void add(T element) { // 제네릭 메서드의 타입 매개변수 T는 서로 다르다.
				...
		}
}
```

```java
Basket<String> basket = new Bakset<>(); // 위 예제의 1의 T가 String으로 지정됩니다. 
basket.<Integer>add(10);                // 위 예제의 2의 T가 Integer로 지정됩니다. 
basket.add(10);                         // 타입 지정을 생략할 수도 있습니다.
```

- 제네릭 클래스와는 제네릭 메서드에는 `static` 메서드에서도 선언할 수 있다.

```java
class Chicken {
	static <T> int setPrice(T element) {
			...
	}
}
```

- 제네릭 메서드가 호출되는 시점에 제네릭 타입이 결정되기 때문에 제네릭 메서드를 정의하는 시점에는 `length()` 와 같은 `String` 클래스의 메서드는 사용할 수 없다.

```java
class Chicken {
	static <T> void setPrice(T element) {
			System.out.println(element.length());  // 불가능
	}
}
```

## 와일드 카드

> 어떠한 타입으로든 대체될 수 있는 타입 파라미터
> 
- `?` 로 사용하며 `extends`와 `super` 를 조합하여 사용한다.

```java
<? extends T>
<? super T>
```

- `<? extends T>` : `T`와 `T`를 상속 받는 하위 클래스 타입만 타입 파라미터로 받을 수 있도록 지정
- `<? super T>` : `T`와 `T`의 상위 클래스만 타입 파라미터로 받도록 지정
- `<?>` : 모든 클래스 타입을 타입 파라미터로 받을 수 있다. `<? extends Object>` 와 같다.