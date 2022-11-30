# 목차
* [목차](#목차)
* [스케줄러(Scheduler)](#스케줄러scheduler)
    + [스케줄러란?](#스케줄러란)
    + [Scheduler 전용 Operator](#scheduler-전용-operator)
    + [subscribeOn()과 publishOn()](#subscribeon과-publishon)

# 스케줄러(Scheduler)

## 스케줄러란?

> Scheduler는 Reactor Sequence 상에서 처리되는 동작들을 하나 이상의 쓰레드에서 동작하도록 별도의 쓰레드를 제공해준다.
> 
- Reactor의 Scheduler를 복잡한 멀티쓰레딩 프로세스를 단순하게 해준다. (멀티 쓰레딩에서 발생할 수 있는 여러 문제점들에 대비하기 위한 복잡한 처리를 직접 할 필요가 없다.)
- Reactor는 기본적으로 Non-Blocking 통신을 위한 비동기 프로그래밍을 위해 탄생했기 때문에 여러 쓰레드를 손쉽게 관리해주는 Scheduler는 Reactor에서 중요한 역할을 한다고 볼 수 있다.

## Scheduler 전용 Operator

- Reactor에서는 Scheduler를 위한 별도의 Operator를 제공한다.
- 즉, 적절한 상황에 맞는 쓰레드를 추가로 생성하는 Operator라고 이해하면 된다.
- 어떤 상황에서 Scheduler를 사용할 수 있는지 예제 코드로 알아보자

### scheduler를 추가하지 않은 Sequence의 쓰레드

```java
package com.codestates.example.schedulers;

import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

/**
 * Scheduler를 추가하지 않을 경우
 */
@Slf4j
public class SchedulersExample01 {
    public static void main(String[] args) {
        Flux
            .range(1, 10)
            .filter(n -> n % 2 == 0)
            .map(n -> n * 2)
            .subscribe(data -> log.info("# onNext: {}", data));
```

```java
13:58:32.313 [main] INFO com.codestates.example.schedulers.SchedulersExample01 - # onNext: 4
13:58:32.315 [main] INFO com.codestates.example.schedulers.SchedulersExample01 - # onNext: 8
13:58:32.315 [main] INFO com.codestates.example.schedulers.SchedulersExample01 - # onNext: 12
13:58:32.315 [main] INFO com.codestates.example.schedulers.SchedulersExample01 - # onNext: 16
13:58:32.315 [main] INFO com.codestates.example.schedulers.SchedulersExample01 - # onNext: 20
```

- 실행 결과를 보면 main 쓰레드에서 실행이 되었음을 알 수 있다.

### SubscribeOn() Operator

- `subscribeOn()` Operator를 추가해서 쓰레드를 하나 더 생성해보자

```java
package com.codestates.example.schedulers;

import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

@Slf4j
public class SchedulersExample02 {
    public static void main(String[] args) throws InterruptedException {

        Flux
                .range(1, 10)
                .subscribeOn(Schedulers.boundedElastic())
                .doOnSubscribe(subscription -> log.info("# doOnSubscribe"))
                .filter(n -> n % 2 == 0)
                .map(n -> n * 2)
                .subscribe(data -> log.info("# onNext: {}", data));

        Thread.sleep(100L);
    }
}
```

```java
14:04:57.355 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
14:04:57.391 [main] INFO com.codestates.example.schedulers.SchedulersExample02 - # doOnSubscribe
14:04:57.395 [boundedElastic-1] INFO com.codestates.example.schedulers.SchedulersExample02 - # onNext: 4
14:04:57.397 [boundedElastic-1] INFO com.codestates.example.schedulers.SchedulersExample02 - # onNext: 8
14:04:57.397 [boundedElastic-1] INFO com.codestates.example.schedulers.SchedulersExample02 - # onNext: 12
14:04:57.397 [boundedElastic-1] INFO com.codestates.example.schedulers.SchedulersExample02 - # onNext: 16
14:04:57.397 [boundedElastic-1] INFO com.codestates.example.schedulers.SchedulersExample02 - # onNext: 20
```

- `subscribeOn()` Operator 내부에 `Schedulers.boundedElastic()` 같은 Scheduler를 지정하면 구독 직후에 실행되는 쓰레드가 main 쓰레드에서 Scheduler로 지정한 쓰레드로 바뀌게 된다.
- `doOnSubscribe()` Operator : 구독 발생 직후에 트리거되는 Operator로써 구독 직후에 어떤 동작을 수행하고 싶다면 doOnSubscribe()에 로직을 작성하면 된다.
- `subscribeOn()` : 구독 직후 실행되는 Opertor 체인의 실행 쓰레드를 Scheduler로 지정한 쓰레드로 변경한다.

### publisherOn() Operator

```java
package com.codestates.example.schedulers;

import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

@Slf4j
public class SchedulersExample03 {
    public static void main(String[] args) throws InterruptedException {
        Flux
                .range(1, 10)
                .subscribeOn(Schedulers.boundedElastic())
                .doOnSubscribe(subscription -> log.info("# doOnSubscribe"))

                .publishOn(Schedulers.parallel())
                .filter(n -> n % 2 == 0)
                .doOnNext(data -> log.info("# filter doOnNext"))

                .publishOn(Schedulers.parallel())
                .map(n -> n * 2)
                .doOnNext(data -> log.info("# map doOnNext"))

                .subscribe(data -> log.info("# onNext: {}", data));

        Thread.sleep(100L);

    }
}
```

```java
14:16:10.281 [main] INFO com.codestates.example.schedulers.SchedulersExample03 - # doOnSubscribe
14:16:10.289 [parallel-2] INFO com.codestates.example.schedulers.SchedulersExample03 - # filter doOnNext
14:16:10.289 [parallel-2] INFO com.codestates.example.schedulers.SchedulersExample03 - # filter doOnNext
14:16:10.289 [parallel-2] INFO com.codestates.example.schedulers.SchedulersExample03 - # filter doOnNext
14:16:10.289 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # map doOnNext
14:16:10.289 [parallel-2] INFO com.codestates.example.schedulers.SchedulersExample03 - # filter doOnNext
14:16:10.289 [parallel-2] INFO com.codestates.example.schedulers.SchedulersExample03 - # filter doOnNext
14:16:10.289 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # onNext: 4
14:16:10.291 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # map doOnNext
14:16:10.291 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # onNext: 8
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # map doOnNext
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # onNext: 12
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # map doOnNext
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # onNext: 16
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # map doOnNext
14:16:10.292 [parallel-1] INFO com.codestates.example.schedulers.SchedulersExample03 - # onNext: 20
```

- `publishOn()` 을 추가할 때마다 추가한 `publishOn()` 을 기준으로 Downstream 쪽 스레드가 publishOn()에서 Scheduler로 지정한 쓰레드로 변경된다.
- `doNext()` : doNext() 바로 앞에 위치한 Operator가 실행될 때, 트리거 되는 Operator

## subscribeOn()과 publishOn()

- `subscribeOn()` : 구독 시점 직후의 실행 흐름을 다른 쓰레드로 바꾸는데 사용한다. 즉, 데이터 소스에서 데이터를 emit하는 원본 Publisher의 실행 쓰레드를 지정하는 역할을 한다.
- `publishOn()` : 전달 받은 데이터를 가공 처리하는 Operator 앞에 추가해서 실행 쓰레드를 별도로 추가하는 역할을 한다.
- `publishOn()` 은 Operator 앞에 여러번 추가할 경우 별도의 쓰레드가 추가로 생성되지만 `subscribeOn()` 은 여러 번 추가해도 하나의 쓰레드만 추가로 생성된다.
- Reactor에서는 Scheduler를 통해 여러가지 유형의 쓰레드를 지원한다.
    - subscirbeOn()에서는 주로 `Schedulers.boundElastic()` 을 사용한다.
    - publishOn()에서는 주로 `Schedulers.parallel()` 를 사용한다.
    - `Schedulers.boundElastic()` : 수명이 긴 작업에 적합하다. 생성된 스레드 수에 상한선을 두고 동일한 작업을 수행한다.
    - `Schedulers.parallel()` : CPU를 많이 사용하지만 수명이 짧은 작업에 적합하다. 작업을 병렬로 수행할 수 있다.

### 구독 직후의 실행 쓰레드(subscribeOn())과 Operator 체인마다 실행 쓰레드(pulisherOn())를 구분해서 실행할 수 있도록 한 이유는 뭘까?

- Spirng WebFlux 기반의 애플리케이션은 적은 수의 쓰레드로 대량의 쵸엉을 Non-Blocking 방식으로 처리할 수 있는 구조를 가지고 있다.
- 대부분의 요청은 적은 수의 쓰레드로 처리되지만 쓰레드가 직접 복잡한 계산을 수행할 경우에는 응답 처리 시간이 늕어질 수 있다.
- 따라서 복잡한 계산이 필요한 작업의 경우 서버 엔진에서 생성하는 요청 처리 쓰레드가 복잡한 계산 작업으로 인해 응답 지연이 발생하지 않도록 별도의 쓰레드가 필요할 수 있다.
- 이 경우 Scheduler를 통해 별도의 쓰레드를 생성한 후, 복잡한 계산을 수행하도록 할 수 있다.