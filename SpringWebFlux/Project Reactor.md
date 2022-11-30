# 목차
* [목차](#목차)
* [Project Reactor](#project-reactor)
    + [Reactor란?](#reactor란)
    + [Reacotr 특징](#reacotr-특징)
    + [Reacotr 구성요소](#reacotr-구성요소)
        + [리액티브 프로그래밍 기본 구조](#리액티브-프로그래밍-기본-구조)
        + [예제](#예제)

# Project Reactor

## Reactor란?

> Reactor는 리액티브 스트림즈 표준 사양을 구현한 구현체 중 하나이다.
> 
- Reactor는 Spring5부터 지원하는 리액티브 스택에 포함되어 리액티브한 애플리케이션으로 동작하는데 있어 핵심적인 역할을 담당하는 리액티브 프로그래밍을 위한 라이브러리이다.

## Reacotr 특징

- 완전한 Non-Blocking 통신을 지원한다.
- Reactor는 Publisher 타입으로 Mono[0|1]와 Flux[N]이라는 두 가지 타입을 제공한다. 0과 1은 0건 또는 1건의 데이터를 emi 할 수 있음을 의미한다. N은 여러 건의 데이터를 emit할 수 있음을 의미한다.
- 서비스들 간의 통신이 잦은 MSA(Microservice Architecture) 기반 애플리케이션들은 요청 쓰레드가 차단되는 Blocking 통신을 사용하기에 무리가 있다.
- 따라서 기본적으로 Non-Blocking 통신을 완벽하게 지원하는 Reacotr는 MSA 구조에 적합한 라이브러리라고 볼 수 있다.
- Reactor는 Backpressure 전략을 지원해준다.
    - Backpressure : Susbscriber의 처리 속도가 Publisher의 emit 속도를 따라가지 못할 때 적절하게 제어하는 전략
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/eb10c17a-c97b-4889-b443-617ca2f911a1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221130T111633Z&X-Amz-Expires=86400&X-Amz-Signature=a8908d333da7d65f45194be265d96f641803710857c1ca89d580947de5f878bd&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
    
    - 위 그림은 Backpressure가 적용되지 않은 Publisher와 Subscriber이다.
    - 그림처럼 Publisher에서 끊임없이 들어오는 데이터를 emit하는 것과 달리 Subscriber의 처리 속도가 느리면 처리하지 않고 대기하는 데이터가 지속적으로 쌓이게 되고 오버플로우가 발생해서 급기야 시스템이 다운될 수도 있다.

## Reacotr 구성요소

### 리액티브 프로그래밍 기본 구조

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

### 예제

```java
package com.codestates.example;

import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class HelloReactorExample {
    public static void main(String[] args) throws InterruptedException {
        Flux    // (1)
            .just("Hello", "Reactor")               // (2)
            .map(message -> message.toUpperCase())  // (3)
            .publishOn(Schedulers.parallel())       // (4)
            .subscribe(System.out::println,         // (5)
                    error -> System.out.println(error.getMessage()),  // (6)
                    () -> System.out.println("# onComplete"));        // (7)

        Thread.sleep(100L);    // (8)
    }
}
```

- (1) `Flux` : Reactor Sequence의 시작점이다. Flux로 시작한다는 것은 Reactor Sequence가 여러 건의 데이터를 처리함을 의미한다.
- (2) `just()` Operator : 원본 데이터 소스로부터 데이터를 emit하는 Publisher 역할을 한다.
- (3) `map()` Operator : Publisher로부터 전달 받은 데이터를 가공한다. just() Operator에서 emit된 문자를 대문자로 변환하고 있다.
- (4) `publishOn()` Operator :  해당 Operator에 Scheduler를 지정하면 `publishOn()` 를 기준으로 Downstream의 쓰레드가 Scheduler에서 지정한 유형의 쓰레드로 변경된다. 여기서는 Reactor Sequence 상에서 두 개의 쓰레드가 실행된다.
- `subscribe()` Operator는 총 세 개의 람다 표현식을 가지고 있다. 이 람다 표현식은 Subscriber에게 전달되어 각각의 동작을 수행하고 있다.
    - (5) 첫 번째 파라미터는 Publisher가 emit한 데이터를 전달받아서 처리하는 역할을 하고 있다.
    - (6) 두 번째 파라미터는 Reactor Sequence 상에서 에러가 발생한 경우, 해당 에러를 전달 받아서 처리하는 역할을 한다.
    - (7) 세 번째 파라미터는 Reactor Sequence가 종료된 후 어떤 후처리를 하는 역할을 한다.

```java
HELLO
REACTOR
# onComplete 
```

- (8) `Thread.sleep(100L)` : Reactor Sequence에서 Scheduler를 지정하면 main 쓰레드 외에 별도의 쓰레드가 하나 더 생긴다.
    
    Reactor에서 Scheduler로 지정한 쓰레드는 모두 데몬 쓰레드이기 때문에 쓰레드인 main 쓰레드가 종료되면 동시에 종료된다. 
    
    따라서 main 쓰레드를 Thread.sleep(100L)을 통해 0.1초 정도 동작을 지연시키면 그 0.1초 사이에 Scheduler로 지정한 데몬 쓰레드를 통해 Reactor Sequence가 정상 동작을 하게 된다.