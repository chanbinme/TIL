# 목차
* [목차](#목차)
* [람다식(Lamda Expression)](#람다식lamda-expression)
    + [함수형 인터페이스](#함수형-인터페이스)
    + [기본 함수형 인터페이스](#기본-함수형-인터페이스)
        + [Runnable](#runnable)
        + [Supplier<T>](#suppliert)
        + [Consumer<T>](#consumert)
        + [Function<T, R>](#functiont-r)
        + [Predicate<T>](#predicatet)
    + [메서드 참조(Method Reference)](#메서드-참조method-reference)
        + [정적 메서드와 인스턴스 메서드 참조](#정적-메서드와-인스턴스-메서드-참조)
    + [생성자 참조(Constuctor Reference)](#생성자-참조constructor-reference)

# 람다식(Lamda Expression)

> 함수형 프로그래밍 기법을 지원하는 자바의 문법요소
> 
- 메서드를 하나의 식으로 표현한 것으로, 코드를 매우 간결하면서 명확하게 표현할 수 있는 장점이 있다.
- 자바 기존의 객체지향 프로그래밍과 함수형 프로그래밍을 혼합하는 방식으로 보다 효율적인 프로그래밍이 가능해졌다.
- 람다식에서는 반환타입과 메서드 이름을 생략할 수 있다. 이런 특징때문에 익명 함수(anonymous function)이라고 불리기도 한다.

```java
// 기존 메서드
int sum(int num1, int num2) {
	return num1 + num2;
}

// 람다식
(int num1, int num2) -> {
	return num1 + num2;
}
```

- 반환값이 있는 경우에는 return문과 세미콜론(;)을 생략할 수 있다.
- 실행문이 하나만 존재할 때 중괄호{}를 생략할 수 있다.

```java
(int num1, int num2) -> num1 + num2
```

- 매개변수의 타입은 추론이 가능한 경우 생략할 수 있는데, 대부분의 경우 생략 가능하다.

```java
(num1, num2) -> num1 + num2
```

- 매개변수가 하나뿐인 경우에는 괄호()를 생략할 수 있다.

```java
num -> num * num
```

## 함수형 인터페이스

> 1개의 추상 메서드를 갖고 있는 인터페이스
> 
- 람다식은 익명 클래스이다. 여기서 익명 클래스란 **객체의 선언과 생성을 동시에 하여 하나의 객체만을 생성하고, 단 한 번만 사용되는 일회용 클래스**를 말한다.
- 람다식이 객체라고 한다면 이 객체 접근하고 사용하기 위한 참조변수가 필요하다. 이 때 사용하는 문법 요소가 자바의 **함수형 인터페이스(Functional Interface)**이다.
- 람다식도 객체이기 때문에 인터페이스에 정의된 추상메서드를 구현할 수 있다.
- 함수형 인터페이스는 **단 하나의 추상 메서드만 선언**될 수 있다. 람다식과 인터페이스의 메서드가 1:1로 매칭되어야 하기 때문이다.

```java
public class LamdaExample {
	public static void main(String[] args) {
		ExampleFunction exampleFunction = (num1, num2) -> num1 + num2;
		System.out.println(exampleFunction.sum(10, 15));

@FunctionalInterface // 컴파일러가 인터페이스가 바르게 정의되어있는지 확인할 수 있도록 정의
interface ExampleFunction {
	public abstract int sum(int num1, int num2);
}

// 결과
25
```

## 기본 함수형 인터페이스

- 함수형 인터페이스를 매번 정의하기에는 불편하기 때문에 자바에서 라이브러리로 제공하고 있다.
- 기본 함수형 인터페이스마다 만들어진 목적에 맞게 이름을 정했기 때문에 실행 메서드 이름이 다르다.

### Runnable

- 인자를 받지 않고 리턴값도 없는 인터페이스
- `run()` 메서드를 사용한다.

```java
public interface Runnable {
	public abstract void run();
}
```

- 예제

```java
Runnable runnable = () -> System.out.println("run anything!");
runnable.run();

// 결과
// run anything!
```

### Supplier<T>

- 인자를 받지 않고 T타입의 객체를 리턴한다.
- `get()` 메서드를 사용한다.

```java
public interfcae Supplier<T> {
	T get();
}
```

- 예제

```java
Supplier<String> getString = () -> "Happy new year!";
String str = getString.get();
System.out.println(str);

// 결과
// Happy new year!
```

### Consumer<T>

- T 타입의 객체를 인자로 받고 리턴 값은 없다.
- `accept()` 메서드를 사용한다.
- `andThen()` 메서드를 사용하면 두 개 이상의 Consumer를 연속적으로 실행할 수 있다.

```java
public interface Consumer<T> {
	void accept(T t);

	default Consumer<T> andThen(Consumer<? super T> after) {
		Objects.requireNonNull(after);
		return (T t) -> {accept(t); after.accept(t);};
```

- 예제

```java
Consumer<String> str = text -> System.out.println("뭐 먹을래? " + text);
Consumer<String> str2 = text -> System.out.println("뿌링클!");
str.andThen(str2).accept("치킨?");

// 결과
뭐 먹을래? 치킨?
뿌링클!
```

### Function<T, R>

- T타입의 인자를 받고, R타입의 객체를 리턴한다.
- `apply()` 메서드를 사용한다.

```java
public interface Function<T, R> {
    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
```

- 예제

```java
Function<Integer, Integer> multiply = (value) -> value * 2;
Integer result = multiply.apply(3);
System.out.println(result);

// 결과
6
```

- `compose()` 는 `andThen()` 와 비슷하게 두 개의 Function을 실행한다. `andThen()` 과의 다른 점은 `compose()` 는 인자로 전달되는 Function이 먼저 수행되고 그 이후에 호출하는 객체의 Funtion이 수행된다.

```java
Function<Integer, Integer> multiply = (value) -> value * 2;
Function<Integer, Integer> add = (value) -> value + 3;

Function<Integer, Integer> addThenMultiply = multiply.compose(add);

Integer result = addThenMultiply.apply(3);
System.out.println(result);

// 결과
12
```

### Predicate<T>

- T 타입의 인자를 받고 결과로 boolean을 리턴한다.
- `test()` 메서드를 사용한다.
- `and()` 와 `or()` 는 다른 Predicate와 함께 사용된다.
    - `and()` : 두 개의 Predicate가 ture일 때 true 리턴
    - `or()` : 둘 중 하나만 true이면 true 리턴

```java
public interface Predicate<T> {
    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}
```

- 예제

```java
Predicate<Integer> isBiggerThanFive = num -> num > 5;
Predicate<Integer> isLowerThanSix = num -> num < 6;
System.out.println(isBiggerThanFive.and(isLowerThanSix).test(10));
System.out.println(isBiggerThanFive.or(isLowerThanSix).test(10));

// 결과
false
true
```

- `isEqual()` 은 static 메서드로, 인자로 전달되는 객체와 같은지 체크하는 Predicate 객체를 만들어 준다.

```java
Predicate<String> isEquals = Predicate.isEqual("뿌링클");
isEquals.test("뿌링클");

// 결과
true
```

## 메서드 참조(Method Reference)

- 람다식에서 불필요한 매개변수를 제거할 때 주로 사용한다.
- 이미 람다식으로 간단해진 익명 객체를 더욱더 간단하게 사용할 수 있도록 만들어준다.
- 람다식과 마찬가지로 인터페이스의 익명 구현 객체로 생성되므로 인터페이스의 추상 메서드가 어떤 매개 변수를 가지고, 리턴 타입이 무엇인가에 따라 달라진다.

```java
// 기존 람다식
IntBinaryOperator operator = (left, rigth) -> Math.max(left, right);

// 메서드 참조
IntBinaryOperator operator = Math::max;  // 클래스이름::메서드이름
```

- 메서드 참조는 정적 혹은 인스턴스 메서드를 참조할 수 있고 생성자 참조도 가능하다.

### 정적 메서드와 인스턴스 메서드 참조

- 정적 메서드는 클래스 이름 뒤에 `::` 를 붙이고 정적 메서드 이름을 기술하여 사용한다.

```java
클래스이름::메서드이름
```

- 인스턴스 메서드의 경우에는 참조 변수 뒤에 `::` 를 붙이고 인스턴스 메서드 이름을 기술한다.

```java
참조 변수명::메서드이름
```

- 예제

```java
public class Calculator {
  public static int staticMethod(int x, int y) {
                        return x + y;
  }

  public int instanceMethod(int x, int y) {
   return x * y;
  }
}
```

```java
import java.util.function.IntBinaryOperator;

public class MethodReferences {
  public static void main(String[] args) throws Exception {
    IntBinaryOperator operator;

    // 정적 메서드
    operator = Calculator::staticMethod;
    System.out.println("정적메서드 결과 : " + operator.applyAsInt(3, 5));

    // 인스턴스 메서드
    Calculator calculator = new Calculator();
    operator = calculator::instanceMethod;
    System.out.println("인스턴스 메서드 결과 : "+ operator.applyAsInt(3, 5));
  }
}

// 결과
정적메서드 결과 : 8
인스턴스 메서드 결과 : 15
```

## 생성자 참조(Constructor Reference)

- 생성자를 참조한다는 것은 객체 생성을 의미한다. 단순히 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치 가능하다.

```java
(a, b) -> {return new 클래스(a, b);};
```

- 클래스 이름 뒤에 `::` 를 붙이고 `new` 연산자를 사용한다.

```java
// 생성자 참조
클래스::new
```

- 생성자 오버로딩 되어 여러 개가 있을 경우 컴파일러가 **함수형 인터페이스의 추상 메서드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행**한다.
- 해당 생성자가 존재하지 않으면 컴파일 오류가 발생한다.
- 예제

```java
public class Member {
  private String name;
  private String id;

  public Member() {
    System.out.println("Member() 실행");
  }

  public Member(String id) {
    System.out.println("Member(String id) 실행");
    this.id = id;
  }

  public Member(String name, String id) {
    System.out.println("Member(String name, String id) 실행");
    this.id = id;
    this.name = name;
  }

  public String getName() {
    return name;
  }

public String getId() {
    return id;
  }
}
```

```java
import java.util.function.BiFunction;
import java.util.function.Function;

public class ConstructorRef {
  public static void main(String[] args) throws Exception {
    Function<String, Member> function1 = Member::new;
    Member member1 = function1.apply("kimcoding");

    BiFunction<String, String, Member> function2 = Member::new;
    Member member2 = function2.apply("kimcoding", "김코딩");
  }
}

// 결과
Member(String id) 실행
Member(String name, String id) 실행
```