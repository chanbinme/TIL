# 목차
* [목차](#목차)
* [리액티브 프로그래밍](#리액티브-프로그래밍)
    + [리액티브 시스템(Reactive System)이란?](#리액티브-스트림즈reactive-streams란)
    + [리액티브 프로그래밍이란?](#리액티브-프로그래밍이란)
    + [리액티브 프로그래밍의 특징](#리액티브-프로그래밍의-특징)
    + [리액티브 스트림즈(Reactive Streams)란?](#리액티브-스트림즈reactive-streams란)
    + [리액티브 스트림즈의 구현체](#리액티브-스트림즈의-구현체)
    + [명령형 프로그래밍 vs 선언형 프로그래밍](#명령형-프로그래밍-vs-선언형-프로그래밍)
    + [리액티브 프로그래밍의 구조](#리액티브-프로그래밍의-구조)
    + [리액티브 프로그래밍에서 사용되는 용어 정의](#리액티브-프로그래밍에서-사용되는-용어-정의)

# 리액티브 프로그래밍

## 리액티브 시스템(Reactive System)이란?

> 클라이언트의 요청에 대한 응답 대기 시간을 최소화할 수 있도록 요청 쓰레드가 차단되지 않게(Non-Blocking) 클라이언트에게 즉각적으로 반응하도록 구성된 시스템
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/bf93b990-b3de-4483-b9d5-e2585ea7512a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221129%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221129T051303Z&X-Amz-Expires=86400&X-Amz-Signature=7f940c540e30f4a247a4aed27a74e2fa7ba54112ea94df2268b45ac6d1e3e12e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

### MEANS

- MEANS는 리액티브 시스템을 사용하는 커뮤니케이션 수단을 의미한다.
    - Message Driven : 리액티브 시스템에서는 메시지 기반 통신을 통해 여러 시스템 간에 느슨한 결합을 유지한다.

### FORM

- FORM은 메시지 기반 통신을 통해 리액티브 시스템이 어떤 특성을 가지는 구조로 형성되는지를 의미한다.
    - Elastic : 시스템으로 들어오는 요청량이 적거나 많거나에 상관없이 일정한 응답성을 유지하는 것
    - Resillient : 시스템의 일부분에 장애가 발생하더라도 응답성을 유지하는 것

### VALUE

- 리액티브 시스템의 핵심 가치가 무엇인지를 표현하는 영역
    - Responsive : 리액티브 시스템은 클라이언트의 요청에 즉각적으로 응답할 수 있어야 한다.
    - Maintable : 클라이언트의 요청에 대한 즉각적인 응답이 지속 가능해야 한다.
    - Extensible : 클라이언트의 요청에 대한 처리량을 자동으로 확장하고 축소할 수 있어야 한다.

## 리액티브 프로그래밍이란?

> 리액티브 시스템에서 사용되는 프로그래밍 모델을 의미한다.
> 
- 리액티브 시스템에서의 메시지 기반 통신(Message Driven)은 Non-Blocking 통신과 유기적인 관계를 맺고 있다.
- 리액티브 프로그래밍은 Non-Blocking 통신을 위한 프로그래밍 모델이다.

## 리액티브 프로그래밍의 특징

- 리액티브 프로그래밍은 선언형 프로그래밍 방식을 사용하는 대표적인 프로그래밍 모델이다.
- 리액티브 프로그래밍에서는 지속적으로 데이터를 입력 받을 수 있고, 리액티브 프로그래밍에서는 데이터가 지속적으로 발생하는 것 자체를 데이터에 어떤 변경이 발생했다고 받아들인다. 이러한 변경 자체를 이벤트로 간주하고, 이벤트가 발생할 때마다 데이터를 계속해서 전달한다.
- 지속적으로 발생하는 데이터를 하나의 데이터 플로우로 보고 데이터를 자동으로 전달한다.

## 리액티브 스트림즈(Reactive Streams)란?

> 리액티브 스트림즈란 리액티브 프로그래밍을 위한 표준 사양(Specification)이다.
> 
- Java에서는 어떤 기술의 표준 사야을 코드로 정의할 경우 일반적으로 Java의 인터페이스로 정의한다.

### 리액티브 스트림즈 컴포넌트

✔️ **Publisher** : 데이터 소스로부터 데이터를 내보내는(emit) 역할을 한다.

```java
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
```

- `publisher` 인터페이스는 `subscribe()` 추상 메서드를 포함하고 있으며, `subscribe()` 의 파라미터로 전달되는 Subscriber가 Publisher로부터 내보내진 데이터를 소비하는 역할을 한다.
- `subscribe()` : Publisher가 내보내는 데이터를 수신할지 여부를 결정한다.  subscribe()가 호출되지 않으면 Publisher가 데이터를 내보내는 프로세스는 시작되지 않는다.
- `emit` : Publisher가 데이터를 내보내는 것

**✔️ Subscriber : Publisher로부터 내보내진 데이터를 소비하는 역할**

```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s);
    public void onNext(T t);
    public void onError(Throwable t);
    public void onComplete();
}
```

- `onSubscribe(Subscription s)`
    
    구독이 시작되는 시점에 호출되며, onSubscribe() 내에서 Publisher에게 요청할 데이터의 개수를 지정하거나 구독 해지 처리할 수 있다.
    
- `onText(T t)`
    
    Publisher가 데이터를 emit할 때 호출되며, emit된 데이터를 전달 받아서 소비할 수 있다.
    
- `onError(Throwable t)`
    
    Publisher로부터 emit된 데이터가 Subscriber에게 전달되는 과정에서 에러가 발생할 경우 호출된다.
    
- `onComplete()`
    
    Publisher가 데이터를 emit하는 과정이 종료될 경우 호출되며, 데이터의 emit이 정상적으로 완료된 후, 처리해야 될 작업이 있으면 onComplete() 내에서 수행할 수 있다.
    

✔️ **Subscription : Subscriber의 구독 자체를 표현한 인터페이스이다.**

```java
public interface Subscription {
    public void request(long n);
    public void cancel();
}
```

- `request(long n)`
    
    Publisher가 emit하는 데이터의 개수를 요청한다.
    
- `cancel()`
    
    구독을 해지하는 역할을 한다. 즉, 구독 해지가 발생하면 Publisher는 더이상 데이터를 emit하지 않는다.
    

✔️ **Processor : Subscriber 인터페이스와 Publisher 인터페이스를 상속하고 있기 때문에 Publisher와 Subscriber의 역할을 동시에 할 수 있다.**

```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```

## 리액티브 스트림즈의 구현체

### Project Reactor

- Project Reactor는 리액티브 스트림즈를 구현한 대표적인 구현체로써 Spring과 궁합이 가장 잘 맞는 리액티브 구현체이다.
- Project Reactor는 Spring 5의 리액티브 스택에 포함되어 있으며 Spring Reactive Application 구현에 있어 핵심적인 역할을 담당하고 있다.

### RxJava

- RxJava는 .NET 기반의 리개티브 라이브러리를 넷플릭스에서 Java 언어로 포팅한 JVM 기반의 리액티브 확장 라이브러리이다.
- RxJava 2.0부터 리액티브 스트림즈 표준 사양을 준수하고 있으며, 이 전 버전의 컴포넌트와 함께 혼용되어 사용되고 있다.

### Java Flow API

- Java 9부터 리액티브 스트림즈를 지원하고 있다.
- Java Flow API는 리액티브 스트림즈를 구현한 구현체가 아니라 리액티브 스트림즈 표준 사양을 Java 안에 포함을 시킨 구조라고 볼 수 있다.
- 즉, 다양한 벤더들이 JDBC API를 구현한 드라이버를 제공할 수 있도록 SPI(Service Provider Interface) 역할을 하는 것처럼 Flow API 역시 리액티브 스트림즈 사양을 구현한 여러 구현체들에 대한 SPI 역할을 한다.

## 명령형 프로그래밍 vs 선언형 프로그래밍

### 명령형 프로그래밍

```java
public class ImperativeProgrammingExample {
    public static void main(String[] args){
        // List에 있는 숫자들 중에서 4보다 큰 짝수의 합계 구하기
        List<Integer> numbers = List.of(1, 3, 6, 7, 8, 11);
        int sum = 0;

        for(int number : numbers){
            if(number > 4 && (number % 2 == 0)){
                sum += number;
            }
        }

        System.out.println(sum);
    }
}
```

- 명령형 프로그래밍 방식은 코드가 어떤식으로 실행되어야 하는지에 대한 구체적인 로직들이 코드 안에 드러난다.

### 선언형 프로그래밍

```java
public class DeclarativeProgramingExample {
    public static void main(String[] args){
        // List에 있는 숫자들 중에서 4보다 큰 짝수의 합계 구하기
        List<Integer> numbers = List.of(1, 3, 6, 7, 8, 11);

        int sum =
                numbers.stream()
                        .filter(number -> number > 4 && (number % 2 == 0))
                        .mapToInt(number -> number)
                        .sum();

        System.out.println("# 선언형 프로그래밍: " + sum);
    }
}
```

- Java에서 선언형 프로그래밍 방식을 이해하기 위한 가장 적절한 예는 Java8부터 진원하는 Stream API이다.
- 코드 상에 보이지 않는 내부 반복자가 명령형 프로그래밍 방식에서 사용하는 for문을 대체하고 있다.
- filter()메서드가 if문을 대신해서 만족하는 숫자를 필터링하고 있다.

## 리액티브 프로그래밍의 구조

```java
package com.codestates.example;

import reactor.core.publisher.Mono;

// 리액티브 프로그래밍 기본 구조
public class HelloReactiveExample01 {
    public static void main(String[] args) {
        // (1) Publisher의 역할
        Mono<String> mono = Mono.just("Hello, Reactive");

        // (2) Subscriber의 역할
        mono.subscribe(message -> System.out.println(message));
    }
}
```

- Publisher는 데이터를 emit하는 역할을 하며, Subscriber는 Publisher가 emit한 데이터를 전달 받아서 소비하는 역할을 한다.
- 위 코드에서 Publisher의 역할을 하는 것은 `Mono` 이다.
- Subscriber의 역할을 하는 것은 subscribe() 메서드 내부에 정의된 `message -> System.out.println(message)` 이다.
- 이 코드를 메서드 체인 형태로 구성할 수 있다.

```java
package com.codestates.example;

import reactor.core.publisher.Mono;

// 리액티브 프로그래밍 기본 구조
public class HelloReactiveExample02 {
    public static void main(String[] args) {
        Mono
            .just("Hello, Reactive")
            .subscribe(message -> System.out.println(message));
    }
}
```

## 리액티브 프로그래밍에서 사용되는 용어 정의

```java
package com.codestates.example;

import reactor.core.publisher.Flux;

import java.util.List;

public class ReactiveGlossaryExample {
    public static void main(String[] args) {
        Flux
            .fromIterable(List.of(1, 3, 6, 7, 8, 11))
            .filter(number -> number > 4 && (number % 2 == 0))
            .reduce((n1, n2) -> n1 + n2)
            .subscribe(System.out::println);

    }
}
```

### Publisher

- Publisher는 데이터를 내보내는 주체를 의미한다. 위 코드에서 Flux가 Publisher이다.

### Emit

- Publisher가 데이터를 내보내는 것을 Emit이라고 한다.

### Subscriber

- Publisher가 emit한 데이터를 전달 받아서 소비하는 주체를 의미한다.
- 위 코드에서 `System.out::println` 가 Subscriber에 해당된다.
- 람다 표현식을 메서드 레퍼런스로 축약하지 않았다면 람다 표현식 자체가 Subscriber에 해당된다.

### Subscribe

- Subscribe는 구독을 의미한다.
- `subscribe()` 메서드를 호출하면 구독을 하는 것이다.

### Signal

- Signal은 Publisher가 발생시키는 이벤트를 의미한다.
- 리액티브 프로그래밍 관련 문서를 보다보면 Signal이라는 용어를 굉장히 만힝 볼 수 있다.
- 위 코드에서 subscribe() 메서드가 호출되면 Publisher인 Flux는 숫자 데이터를 하나씩 emit한다. 이 때, 숫자 데이터를 하나씩 emit하는 자체를 리액티브 프로그래밍에서는 이벤트가 발생하는 것으로 간주된다. 이 이벤트 발생을 다른 컴포넌트에게 전달하는 것을 Signal을 전송한다라고 표현한다.

### Operator

- Operator는 리액티브 프로그래밍에서 어떤 동작을 수행하는 메서드를 의미한다.
- fromIteratoable(), filter(), reduce() 등 메서드 하나 하나를 Operator라고 한다.

### Sequence

- Sequence는 Operator 체인으로 표현되는 데이터의 흐름을 의미한다.
- Operator 체인으로 작성된 코드 자체를 하나의 Sequence라고 할 수 있다.

### Upstream/Downstream

- Sequence 상의 특정 Operator를 기준으로 위쪽 Sequence 일부를 Upstream이라고 하며, 아래 쪽 Sequence 일부를 DownStream이라고 표현한다.
- 위 코드에서 filter() 위쪽의 fromIteratble()은 Upstream이고, filter() 아래 쪽의 reduce()는 Downstream이 된다.

## 정리

- 리액티브 시스템은 클라이언트의 요청에 반응을 잘하는 시스템을 말한다.
- 리액티브 프로그래밍은 리액티브 시스템에서 사용되는 프로그래밍 모델이다.
- 리액티브 스트림즈는 리액티브 프로그래밍을 위한 표준 사양이다.
- 리액티브 스트림즈는 네 개의 컴포넌트로 구성된다.
    - Publisher
    - Subscriber
    - Subscription
    - Processor