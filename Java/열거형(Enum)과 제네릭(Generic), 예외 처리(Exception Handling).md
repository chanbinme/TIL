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
* [예외 처리(Exception Handling)](#예외-처리exception-handling)
    + [컴파일 에러(Compile Time Error)](#컴파일-에러compile-time-error)
    + [런타임 에러(Runtime Error)](#런타임-에러run-time-error)
    + [에러와 예외](#에러와-예외)
    + [예외 클래스의 상속 계층도](#예외-클래스의-상속-계층도)
        + [일반 예외 클래스](#일반-예외-클래스exception)
        + [실행 예외 클래스](#실행-예외-클래스runtime-exception)
    + [try-catch문](#try-catch문)

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

# 예외 처리(Exception Handling)

> 프로그램의 예외를 예상하고 미리 예측해 비정상적인 종료를 방지하고, 정상적인 실행 상태를 유지할 수 있도록 처리하는 코드 작성 과정
> 
- 에러가 발생하는 원인은 크게 내부적인 요인과 외부적인 요인을 구분할 수 있다.
    - 외부 요인 : 하드웨어의 문제, 네트워크의 연결 끊김, 사용자 조작 오류 등
    - 내부 요인 : 개발자의 코드 에러
- 예외 처리는 내부 요인인 개발자의 코드 에러를 미리 방지하는 것이다.
- 에러는 크게 컴파일 에러와 런타임 에러로 구분할 수 있다.

## 컴파일 에러(Compile Time Error)

> 컴파일 할 때 발생하는 에러
> 
- 주로 세모콜론 생략, 오탈자, 잘못된 자료형, 잘못된 포맷 등 문법적인 문제를 가리킨다. 신택스 에러(Syntax Errors)라고 부르기도 한다.
- 컴파일 에러는 자바 컴파일러가 오류를 감지하여 알려주기 때문에 상대적으로 쉽게 발견하고 수정할 수 있다.

```java
public class ErrorTest {
    public static void main(String[] args) {
        int i;

        for (i= 1; i<= 5; i++ { // ')' 괄호 빼먹음
            System.out.println(i);
        }
    }
}

// 결과
java: ')' expected
```

## 런타임 에러(Run Time Error)

> 코드를 실행하는 과정, 즉 런타임 시에 발생하는 에러
> 
- 개발자가 컴퓨터가 수행할 수 없는 특정한 작업을 요청할 때 발생
- 런타임 에러는 프로그램이 실행될 때 자바 가상 머신(JVM; Java Virtual Marchine)에 의해 감지된다.
- 컴파일 중에는 문제가 있는지 알 수가 없다.

```java
public class RuntimeErrorTest {

    public static void main(String[] args) {
        System.out.println(4 * 4);
        System.out.println(4 / 0); // 예외 발생
    }
}

//출력값
16
// 특정 숫자를 0으로 나눴을 때 발생하는 ArithmeticException 예외 발생
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at RuntimeErrorTest.main(RuntimeErrorTest.java:5)
```

## 에러와 예외

- 자바에서는 프로그래 오류를 크게 에러(error)와 예외(Exception)으로 구분하고 있다.
    - 에러 : 복구하기 어려운 수준의 심각한 오류(메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverflowError) 등)
    - 예외 : 잘못된 사용 또는 코딩으로 인한 상대적으로 미약한 수준의 오류. 코드 수정 등을 통해 수습이 가능한 오류를 지칭한다.

## 예외 클래스의 상속 계층도

- 자바에서는 예외가 발생하면 예외 클래스로부터 객체를 생성하여 해당 인스턴스를 통해 예외 처리를 한다.
- 모든 예외의 최고 상위 클래스는 `Exception` 클래스이며, 다시 크게 일반 예외 클래스와 실행 예외 클래스로 나눌 수 있다.

### 일반 예외 클래스(Exception)

- 런타임 시 발생하는 `RuntimeException` 클래스와 그 하위 클래스를 제외한 모든 `Exception` 클래스와 그 하위 클래스를 가리킨다.
- 컴파일러가 코드 실행 전에 예외 처리 코드 여부를 검사한다고해서 checked 예외라고 부르기도 한다.
- 주로 잘못된 클래스명(ClassNotFoundException)이나 데이터 형식(DataFormatException) 등 사용자의 실수로 발생하는 경우가 많다.

### 실행 예외 클래스(Runtime Exception)

- `RuntimeException` 클래스와 그 하위 클래스를 가리킨다.
- 컴파일러가 예외 처리 코드 여부를 검사하지 않는다는 의미에서 unchecked 예외라고 부르기도 한다.
- 주로 개발자의 실수로 발생하는 경우가 많고, 자바 문법 요소와 관련이 있다.
- 클래스 간 형변환 오류(ClassCastException), 벗어난 배열 범위 지정(ArrayIndexOfBoundException), 값이 null인 참조변수 사용(NullPointerException) 등이 있다.

## try-catch문

- 자바에서 예외 처리는 `try-catch` 문을 통해 구현할 수 있다.
    - try 블럭 : 예외가 발생할 가능성이 있는 코드를 삽입
    - catch 블럭 : 예외가 발생하는 경우에 실행되는 코드, 여러 종류의 예외를 처리할 수 있다.
    - finally 블럭 : 선택적으로 삽입할 수 있지만, 만약 삽입하는 경우 예외 발생 여부와 상관없이 항상 실행된다.

```java
try {
	// 예외가 발생할 가능성이 있는 코드를 삽입
}
catch (ExceptionType1 e1) {
	// ExceptionType1 유형의 예외 발생 시 실행할 코드
}
catch (ExceptionType2 e2) {
	// ExceptionType2 유형의 예외 발생 시 실행할 코드
}
finally {
	// finally 블럭은 선택적으로 삽입
	// 예외 발생 여부와 상관없이 실행
}
```

- 예외와 일치하는 catch 블럭만 실행되고 예외처리 코드가 종료되거나 finally 블럭으로 넘어간다.

```java
public class RuntimeExceptionTest {

    public static void main(String[] args) {

        try {
            System.out.println("[소문자 알파벳을 대문자로 출력하는 프로그램]");
            printMyName(null); // (1) 예외 발생
            printMyName("abc"); // 이 코드는 실행되지 않고 catch 문으로 이동
        } 
        catch (ArithmeticException e) {
            System.out.println("ArithmeticException 발생!"); // (2) 첫 번째 catch문
        } 
        catch (NullPointerException e) { // (3) 두 번째 catch문
            System.out.println("NullPointerException 발생!"); 
            System.out.println("e.getMessage: " + e.getMessage()); // (4) 예외 정보를 얻는 방법 - 1
            System.out.println("e.toString: " + e.toString()); // (4) 예외 정보를 얻는 방법 - 2
            e.printStackTrace(); // (4) 예외 정보를 얻는 방법 - 3
        } 
        finally {
            System.out.println("[프로그램 종료]"); // (5) finally문
        }
    }

    static void printMyName(String str) {
        String upperCaseAlphabet = str.toUpperCase();
        System.out.println(upperCaseAlphabet);
		}
}

// 결과
[소문자 알파벳을 대문자로 출력하는 프로그램]
NullPointerException 발생!
e.getMessage: null
e.toString: java.lang.NullPointerException
[프로그램 종료]
java.lang.NullPointerException
	at RuntimeExceptionTest.printMyName(RuntimeExceptionTest.java:20)
	at RuntimeExceptionTest.main(RuntimeExceptionTest.java:7)
```
