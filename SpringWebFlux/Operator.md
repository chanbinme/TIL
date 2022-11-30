# 목차
* [목차](#목차)
* [Operators](#operators)
    + [상황별로 분류된 Operator 목록 리뷰](#상황별로-분류된-operator-목록-리뷰)
    + [새로운 Sequence를 생성(Creating)하고자 할 경우](#새로운-sequence를-생성creating하고자-할-경우)
    + [기존 Sequence에서 변환 작업(Transforming)이 필요한 경우](#기존-sequence에서-변환-작업transforming이-필요한-경우)
    + [Sequence 내부의 동작을 확인(Peeking)하고자 할 경우](#sequence-내부의-동작을-확인peeking하고자-할-경우)
    + [에러를 처리(Handling errors)하고자 할 경우](#에러를-처리handling-errors하고자-할-경우)
    


# Operators

- 리액티브 프로그래밍은 Operator에서 시작해서 Operator로 끝난다고 해도 과언이 아닐만큼 Operator는 Reactor에서 가장 중요한 구성요소이다.
- 리액티브 스트림즈의 구현체인 Reactor 역시 다양한 종류의 Operator를 지원한다.
- Reactor에서 지원하는 Operator는 너무 많기 때문에 자주 사용하는 Operator에 대해서 알아보자

## **상황별로 분류된 Operator 목록 리뷰**

- **새로운 Sequence를 생성(Creating)하고자 할 경우**
    - `just()`
    - ⭐ `fromStream()`
    - ⭐ `fromIterable()`
    - `fromArray()`
    - `range()`
    - `interval()`
    - `empty()`
    - `never()`
    - `defer()`
    - `using()`
    - `generate()`
    - ⭐ `create()`
- **기존 Sequence에서 변환 작업(Transforming)이 필요한 경우**
    - ⭐ `map()`
    - ⭐ `flatMap()`
    - ⭐ `concat()`
    - `collectList()`
    - `collectMap()`
    - `merge()`
    - ⭐ `zip()`
    - `then()`
    - `switchIfEmpty()`
    - `and()`
    - `when()`
- **Sequence 내부의 동작을 확인(Peeking)하고자 할 경우**
    - `doOnSubscribe`
    - ⭐`doOnNext()`
    - `doOnError()`
    - `doOnCancel()`
    - `doFirst()`
    - `doOnRequest()`
    - `doOnTerminate()`
    - `doAfterTerminate()`
    - `doOnEach()`
    - `doFinally()`
    - ⭐`log()`
- **Sequence에서 데이터 필터링(Filtering)이 필요한 경우**
    - ⭐`filter()`
    - `ignoreElements()`
    - `distinct()`
    - ⭐`take()`
    - `next()`
    - `skip()`
    - `sample()`
    - `single()`
- **에러를 처리(Handling errors)하고자 할 경우**
    - ⭐`error()`
    - ⭐`timeout()`
    - `onErrorReturn()`
    - `onErrorResume()`
    - `onErrorMap()`
    - `doFinally()`
    - ⭐`retry()`

### Operator 예제를 위한 샘플 데이터

```java
public class SampleData {
    public static List<Coffee> coffeeList = List.of(
            new Coffee("아메리카노", "Americano", 2500, "AMR"),
            new Coffee("카페라떼", "CafeLatte", 3500, "CFR"),
            new Coffee("바닐라 라떼", "Vanilla Latte", 4500, "VNL"),
            new Coffee("카라멜 마끼아또", "Caramel Macchiato", 5500, "CRM"),
            new Coffee("에스프레소", "Espresso", 5000, "ESP")
    );

    // A 지점 카페의 월별 매출
    public static final List<Integer> salesOfCafeA = Arrays.asList(
            5_500_000, 4_200_000, 3_500_000, 5_000_000, 3_700_000, 4_000_000, 5_300_000, 5_800_000,
            3_500_000, 2_900_000, 5_400_000, 4_900_000
    );

    // B 지점 카페의 월별 매출
    public static final List<Integer> salesOfCafeB = Arrays.asList(
            2_500_000, 3_100_000, 4_300_000, 3_500_000, 3_200_000, 2_800_000, 3_100_000, 4_200_000,
            3_100_000, 3_200_000, 3_400_000, 4_100_000
    );

    // C 지점 카페의 월별 매출
    public static final List<Integer> salesOfCafeC = Arrays.asList(
            5_500_000, 5_100_000, 5_300_000, 5_500_000, 4_700_000, 4_800_000, 4_100_000, 5_200_000,
            5_100_000, 4_200_000, 4_400_000, 5_100_000
    );
}
```

## 새로운 Sequence를 생성(Creating)하고자 할 경우

### `fromStream`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6624d963-c080-4bc6-9aa0-761c777a2a6a/Untitled.png)

- `fromStream()` Operator : Java의 Stream을 입력으로 전달 받아 emit하는 Operator이다.

```java
import reactor.core.publisher.Flux;

import java.util.stream.Stream;

public class FromStreamExample01 {
    public static void main(String[] args) {
        Flux
                .fromStream(Stream.of(200, 300, 400, 500, 600))
                .reduce((a, b) -> a + b)
                .subscribe(System.out::println);
    }
}
```

```java
2000
```

### `fromIterable()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c24a0bff-ec78-41b1-96f6-384c71eff96b/Untitled.png)

`fromIterable()` Operator : Java의 Iterable을 입력으로 전달 받아 emit하는 Operator이다. List, Map, Set 등의 컬렉션을 `fromIterable()` 파라미터로 전달할 수 있다.

```java
import com.codestates.example.operators.sample_data.SampleData;
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

@Slf4j
public class FromIterableExample01 {
    public static void main(String[] args) {
        Flux
                .fromIterable(SampleData.coffeeList)
                .subscribe(coffee -> log.info("{} : {}", coffee.getKorName(), coffee.getPrice()));
    }
}
```

```java
14:54:07.889 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
14:54:07.900 [main] INFO com.codestates.example.operators.create.FromIterableExample01 - 아메리카노 : 2500
14:54:07.902 [main] INFO com.codestates.example.operators.create.FromIterableExample01 - 카페라떼 : 3500
14:54:07.902 [main] INFO com.codestates.example.operators.create.FromIterableExample01 - 바닐라 라떼 : 4500
14:54:07.902 [main] INFO com.codestates.example.operators.create.FromIterableExample01 - 카라멜 마끼아또 : 5500
14:54:07.902 [main] INFO com.codestates.example.operators.create.FromIterableExample01 - 에스프레소 : 5000
```

### `create()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20e1f8fa-d09b-4b6a-98bb-2c955d7fe892/Untitled.png)

- `create()` Operator : 프로그래밍 방식 중 하나로 Signal 이벤트를 발생시키는 Operator이다. 한 번에 여러 건의 데이터를 비동기적으로 emit할 수 있다.
- Signal이라는 용어는 Publisher가 발생 시키는 이벤트를 의미한다. 일반적으로 Publisher가 데이터를 emit할 경우, onNext Signal 이벤트를 전송한다고 표현한다.
- 마블 다이어그램 상단의 ‘multithreaded source’는 여러개의 쓰레드에서 데이터를 비동기적으로 emit하는 것을 의미한다.

```java

import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;
import reactor.core.publisher.FluxSink;

import java.util.Arrays;
import java.util.List;

@Slf4j
public class CreateExample {
    private static List<Integer> source= Arrays.asList(1, 3, 5, 7, 9, 11, 13, 15, 17, 19);

    public static void main(String[] args) {
        Flux.create((FluxSink<Integer> sink) -> {   // (1)
            // (2)
            sink.onRequest(n -> {
                for (int i = 0; i < source.size(); i++) {
                    sink.next(source.get(i));   // (3)
                }
                sink.complete();    // (4)
            });

            // (5)
            sink.onDispose(() -> log.info("# clean up"));
        }).subscribe(data -> log.info("# onNext: {}", data));
    }
}
```

- `create()` Operator의 파라미터는 FluxSink라는 람다 파라미터를 가지는 람다 표현식이다.
    
    (1)의 `FluxSink` 라는 Flux나 Mono에서 just(), fromIterable()같은 데이터 생성 Operator에 데이터소스를 전달하면 내부에서 알아서 데이터를 emit하는 등의 Sequence를 진행하는 것이 아니라 프로그래밍 방식으로 직접 Signal 이벤트를 발생시켜서 Sequence를 진행하도록  해주는 기능이다.
    
    따라서 create() Operator 내부에서 FluxSink의 객체를 통해 모든 작업을 진행한다.
    
- (2) `OnRequest()` : Subscriber에서 데이터를 요청하면 onRequest()의 파라미터인 람다 표현식이 실행된다.
    - (3) for문을 순회하면서 `next()` 메서드로 List source의 원소를 emit하고 있다.
    - (4) List source 원소를 모두 emit했으므로, Sequence를 종료하기 위해 `complete()` 을 호출한다.
    - (5) `onDispose()` 는 Sequence가 완전히 종료되기 직전에 호출되며, sequence 종료 직전 후처리 작업을 할 수 있다.

```java
15:06:30.246 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
15:06:30.263 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 1
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 3
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 5
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 7
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 9
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 11
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 13
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 15
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 17
15:06:30.265 [main] INFO com.codestates.example.operators.create.CreateExample - # onNext: 19
15:06:30.267 [main] INFO com.codestates.example.operators.create.CreateExample - # clean up
```

## 기존 Sequence에서 변환 작업(Transforming)이 필요한 경우

### `flatMap()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b08690f-f537-407b-9850-702b71b7e1fe/Untitled.png)

- `flatMap()` Operator : `flatMap()` 바깥 쪽에서 하나의 데이터가 들어올 때마다 새로운 Sequence가 생성된다. 즉, `flatMap()` 내부로 들어오는 데이터 한 건당 하나의 Sequence가 생성된다.
- 예를 들어 Upstream에서 2개의 데이터를 emit하고, flatMap() 내부에서 3개의 데이터를 emit하는 Sequence가 있다면 DownStream으로 emit되는 데이터는 총 6개(2 x 3)이다.
- `flatMap()` 내부에서 정의하는 Sequence를 Inner Sequence라고 부른다.

```java
package com.codestates.example.operators.transformation;

import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

/**
 * flatMap() 기본 예제
 */
@Slf4j
public class FlatMapExample01 {
    public static void main(String[] args) throws InterruptedException {
        Flux
            .range(2, 6)         // (1)
            .flatMap(dan -> Flux
                    .range(1, 9)  // (2)
                    .publishOn(Schedulers.parallel())   // (3)
                    .map(num -> dan + " x " + num + " = " + dan * num)) // (4)
            .subscribe(log::info);

        Thread.sleep(100L);
    }
}
```

```java
15:22:17.637 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
15:22:17.731 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 1 = 7
15:22:17.732 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 2 = 14
15:22:17.734 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 3 = 21
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 4 = 28
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 5 = 35
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 6 = 42
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 7 = 49
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 8 = 56
15:22:17.735 [parallel-6] INFO com.codestates.example.operators.create.FlatMapExample - 7 x 9 = 63
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 1 = 2
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 2 = 4
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 3 = 6
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 4 = 8
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 5 = 10
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 6 = 12
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 7 = 14
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 8 = 16
15:22:17.738 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 2 x 9 = 18
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 1 = 3
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 2 = 6
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 3 = 9
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 4 = 12
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 5 = 15
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 6 = 18
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 7 = 21
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 8 = 24
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 3 x 9 = 27
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 1 = 4
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 1 = 5
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 2 = 10
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 3 = 15
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 4 = 20
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 5 = 25
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 6 = 30
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 7 = 35
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 8 = 40
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 5 x 9 = 45
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 1 = 6
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 2 = 12
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 3 = 18
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 4 = 24
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 5 = 30
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 6 = 36
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 7 = 42
15:22:17.739 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 8 = 48
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 6 x 9 = 54
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 2 = 8
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 3 = 12
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 4 = 16
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 5 = 20
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 6 = 24
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 7 = 28
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 8 = 32
15:22:17.740 [parallel-3] INFO com.codestates.example.operators.create.FlatMapExample - 4 x 9 = 36-
```

- `flatMap()` Operator를 이해하기 좋은 예제가 구구단이다.
- (1) range() Operator를 이용해서 구구단 2단부터 7단까지 출력하도록 지정한다.
- (2) flatMap() 내부에서 range()로 하나의 단을 출력하도록 1부터 9까지 숫자를 지정한다.
- (3) flatMap() 내부의 Inner Sequence를 처리할 쓰레드를 할당한다. 전체 Sequence는 여러 개의 쓰레드가 비동기적으로 동작한다.
- 위 결과를 보면 알 수 있듯이 `flatMap()` Operator에서 추가 쓰레드를 할당할 경우, 작업의 처리 순서를 보장하지 않는다는 것을 알 수 있다.

### `concat()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ab1ca49-aad5-415c-81e3-65af214119cb/Untitled.png)

- `concat()` Operator : 입력으로 전달하는 Publisher의 Sequence를 연결해서 차례대로 데이터를 emit한다.
- Java의 String 클래스의 concat() 메서드와 비슷하다.
- concat() Operator는 이어 붙인 Sequence에서 시간 순서대로 데이터를 차례대로 emit한다.

```java
package com.codestates.example.operators.create;

import reactor.core.publisher.Flux;

public class ConcatExample01 {
    public static void main(String[] args) {
        Flux
                .concat(Flux.just("Monday", "Tuesday", "Wednesday", "Thursday", "Friday"),
                        Flux.just("Saturday", "Sunday"))
                .subscribe(System.out::println);
    }
}
```

- concat() Operator의 입력으로 두 개의 Flux Sequence를 전달했기 때문에 두 개의 Sequence를 이어 붙여서 논리적으로 하나의 Sequence로 동작하게 된다.

```java
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday
```

```java
import com.codestates.example.operators.sample_data.SampleData;
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

@Slf4j
public class ConcatExample02 {
    public static void main(String[] args) {
        Flux
                .concat(Flux.fromIterable(SampleData.salesOfCafeA),
                        Flux.fromIterable(SampleData.salesOfCafeB),
                        Flux.fromIterable(SampleData.salesOfCafeC))
                .reduce((a, b) -> a + b)
                .subscribe(data -> log.info("# total sales: {}", data));
    }
}
```

```java
15:34:13.296 [main] INFO com.codestates.example.operators.create.ConcatExample02 - # total sales: 153200000
```

- 3개 카페 지점의 월별 매출액을 모두 하나의 Sequence로 연결 한 다음 카페의 전체 매출액을 계산하는 예제이다.
- reactor-core 모듈에는 수학 계산을 위한 Operator는 존재하지 않기 때문에 `reduce()` Operator를 사용해서 계산했다.
- reactor-extra 모듈에는 수학 계산과 관련된 작업을 처리할 수 있는 `MathFlux` 라는 Reactor 타입을 포함하고 있으며, `MathFlux` 의 Operator를 사용하면 숫자 데이터의 합계나 평균 등을 손쉽게 구할 수 있다.

### `zip()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87f064e9-d612-4a0b-80d4-faa29ba5fee9/Untitled.png)

- `zip()` Operator : 입력으로 전달되는 여러 개의 Publisher Sequence에서 emit된 데이터를 결합하는 Operator
- 여기서 `결합` 의 의미는 Publisher가 emit하는 데이터를 하나씩 전달 받아서 새로운 데이터를 만든 후에 Downstream으로 전달한다는 의미이다.
- 위 마블 다이어그램에는 두 개의 Sequence가 있으며, 위 쪽 Sequence는 알파벳 데이터를 emit하고 있고, 아래쪽 Sequence는 숫자 데이터를 emit하고 있다.
- 중요한 것은 각가의 Sequence에서 emit되는 데이터 중에서 같은 차례(index)의 데이터들이 결합된다.
- 각 Sequence에서 emit되는 데이터의 시점이 다르기 때문에 결합되어야 하는 데이터(같은 index)가 emit이 될 때까지 기다렸다가 결합한다.

```java
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

import java.time.Duration;

@Slf4j
public class ZipExample01 {
    public static void main(String[] args) throws InterruptedException {
        Flux<Long> source1 = Flux.interval(Duration.ofMillis(200L)).take(4);

        Flux<Long> source2 = Flux.interval(Duration.ofMillis(400L)).take(6);

        Flux
                .zip(source1, source2, (data1, data2) -> data1 + data2)
                .subscribe(data -> log.info("# onNext: {}", data));

        Thread.sleep(3000L);
    }
}
```

- `interval()` Operator : 파라미터로 전달한 시간(`Duration.ofMillis(…)`)을 주기로 0부터 1씩 증가한 숫자를 emit하는 Operator이다.
- `interval()` Operator가 끊임없이 숫자를 emit하기때문에 `take()` Operator를 이용해서 `take()` Operator 파라미터로 지정한 숫자만큼의 데이터만 emit한다.
- (1)과 (2)의 Sequence에서 `zip()` Operator의 입력으로 전달되기 때문에 두 Sequence의 emit 시점이 매번 다르더라도 emit 시점이 늦은 데이터가 emit될 때까지 대기했다가 두 개의 데이터를 전달 받게 된다.

```java
15:43:59.552 [parallel-2] INFO com.codestates.example.operators.create.ZipExample01 - # onNext: 0
15:43:59.951 [parallel-2] INFO com.codestates.example.operators.create.ZipExample01 - # onNext: 2
15:44:00.351 [parallel-2] INFO com.codestates.example.operators.create.ZipExample01 - # onNext: 4
15:44:00.750 [parallel-2] INFO com.codestates.example.operators.create.ZipExample01 - # onNext: 6
```

- (2)의 Sequence가 총 6개의 데이터를 emit하지만 (1) Sequence가 4개의 데이터만 emit하고 종료되기 때문에 (2)의 Sequence는 결합할 대상이 없으므로 남은 두 개의 데이터는 폐기 된다.

## Sequence 내부의 동작을 확인(Peeking)하고자 할 경우

### `doOnNext()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bcad346-7195-4317-944b-f161d14db364/Untitled.png)

- `doOnNext()` Operator : 데이터 emit 시 트리거되어 부수 효과(side-effect)를 추가할 수 있는 Operator
- 마블 다이어그램에서 실제 emit된 데이터를 가지고 무언가 처리 작업을 하지만 Downstream으로 전달되는 것은 emit된 데이터이지 `doOnNext()` Operator에서 처리된 작업의 결과 값은 아니다.
- 부수 효과(side-effect) : 어떤 동작을 실행하되 리턴 값이 없는 것
- `doOnNext()` 는 주로 로깅에 사용되지만 데이터를 emit하면서 필요한 추가 작업이 있다면 `doOnNext()` 에서 처리할 수 있다.

```java
import com.codestates.example.entity.Coffee;
import com.codestates.example.operators.sample_data.SampleData;
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

@Slf4j
public class DoOnNextExample01 {
    public static void main(String[] args) {
        Flux
                .fromIterable(SampleData.coffeeList)
                .doOnNext(coffee -> validateCoffee(coffee))
                .subscribe(data -> log.info("{} : {}", data.getKorName(), data.getPrice()));
    }

    private static void validateCoffee(Coffee coffee) {
        if (coffee == null) {
            throw new RuntimeException("Not found coffee");
        }
    }
}
```

- 위 코드는 `doOnNext()` 를 이용해서 emit되는 데이터의 유효성 검증을 진행하고 있다.

```java
19:37:06.604 [main] INFO com.codestates.example.operators.create.DoOnNextExample01 - 아메리카노 : 2500
19:37:06.606 [main] INFO com.codestates.example.operators.create.DoOnNextExample01 - 카페라떼 : 3500
19:37:06.606 [main] INFO com.codestates.example.operators.create.DoOnNextExample01 - 바닐라 라떼 : 4500
19:37:06.606 [main] INFO com.codestates.example.operators.create.DoOnNextExample01 - 카라멜 마끼아또 : 5500
19:37:06.606 [main] INFO com.codestates.example.operators.create.DoOnNextExample01 - 에스프레소 : 5000
```

### `log()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efd37beb-a637-47ca-922f-27cce71235bb/Untitled.png)

- `log()` Operator : Publisher에서 발생하는 Signal 이벤트를 로그로 출력해주는 역할

```java
import reactor.core.publisher.Flux;

import java.util.stream.Stream;

public class LogExample01 {
    public static void main(String[] args) {
        Flux
                .fromStream(Stream.of(200, 300, 400, 500, 600))
                .log()
                .reduce((a, b) -> a + b)
                .log()
                .subscribe(System.out::println);
    }
}
```

```java
19:41:47.702 [main] DEBUG reactor.util.Loggers - Using Slf4j logging framework
19:41:47.761 [main] INFO reactor.Flux.Stream.1 - | onSubscribe([Synchronous Fuseable] FluxIterable.IterableSubscription)
19:41:47.763 [main] INFO reactor.Mono.Reduce.2 - | onSubscribe([Fuseable] MonoReduce.ReduceSubscriber)
19:41:47.764 [main] INFO reactor.Mono.Reduce.2 - | request(unbounded)
19:41:47.764 [main] INFO reactor.Flux.Stream.1 - | request(unbounded)
19:41:47.764 [main] INFO reactor.Flux.Stream.1 - | onNext(200)
19:41:47.764 [main] INFO reactor.Flux.Stream.1 - | onNext(300)
19:41:47.765 [main] INFO reactor.Flux.Stream.1 - | onNext(400)
19:41:47.765 [main] INFO reactor.Flux.Stream.1 - | onNext(500)
19:41:47.765 [main] INFO reactor.Flux.Stream.1 - | onNext(600)
19:41:47.765 [main] INFO reactor.Flux.Stream.1 - | onComplete()
19:41:47.765 [main] INFO reactor.Mono.Reduce.2 - | onNext(2000)
2000
19:41:47.766 [main] INFO reactor.Mono.Reduce.2 - | onComplete()
```

- 구독 시점에 onSubscribe Signal 이벤트가 발생했다.
- 데이터 요청 시, request Signal 이벤트가 발생했다.
- Publisher가 데이터를 Emit할 때 onNext Signal 이벤트가 발생한다.
- Publisher의 데이터를 emit이 정상적으로 종료되면 onCompleteSignal 이벤트가 발생한다.

## 에러를 처리(Handling errors)하고자 할 경우

### `error()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ece90c5c-5dd4-4e58-b1ec-feec9e61ffd4/Untitled.png)

- `error()` Operator : 의도적으로 onError Signal 이벤트를 발생시킬 때 사용할 수 있는 Operator
- Reactor Sequence 상에서 의도적으로 예외를 던져서 onError Signal 이벤트를 발생시키는데 사용할 수 있다.

```java
import com.codestates.example.entity.Coffee;
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Mono;

@Slf4j
public class ErrorExample01 {
    public static void main(String[] args) {
        Mono.justOrEmpty(findVerifiedCoffe())
                .switchIfEmpty(Mono.error(new RuntimeException("Not found coffee")))
                .subscribe(
                        data -> log.info("{} : {}", data.getKorName(), data.getPrice()),
                        error -> log.error("# onError: {}", error.getMessage())
                );
    }

    private static Coffee findVerifiedCoffe() {
        return null;
    }
}
```

- `justOrEmpty()` Operator : 파라미터로 전달되는 데이터 소스가 null이어도 에러가 발생하지 않는다. `just()` Operator는 null 데이터를 emit하면 에러가 발생한다.
- `switchIfEmpty()` Operator : Upstream에서 전달되는 데이터가 null이면 대체 동작을 수행할 수 있다. 여기서는 onError Signal 이벤트를 발생하도록 했다.

```java
19:51:47.805 [main] ERROR com.codestates.example.operators.create.ErrorExample01 - # onError: Not found coffee
```

### `timeout()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/271bc64e-5158-4dd7-b8bb-43681bd66cc8/Untitled.png)

- `timeout()` Operator : 입력으로 주어진 시간동안 emit되는 데이터가 없으면 onError Signal 이벤트를 발생시킨다.

### `retry()`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e8eb63c-3f49-4829-a4ac-400c58086f2d/Untitled.png)

- `retry()` Operator : Sequence 상에서 에러가 발생할 경우, 입력으로 주어진 숫자만큼 재구독해서 Sequence를 다시 시작한다.
- `timeout()` Operator는 `retry()` Operator와 함께 사용하는 경우가 많기 때문에 두 Operator를 함께 사용하는 예제를 살펴보자

```java
import com.codestates.example.entity.Coffee;
import com.codestates.example.operators.sample_data.SampleData;
import lombok.extern.slf4j.Slf4j;
import reactor.core.publisher.Flux;

import java.time.Duration;
import java.util.stream.Collectors;

@Slf4j
public class TimeoutRetryExample01 {
    public static void main(String[] args) throws InterruptedException {
        getCoffees()
                .collect(Collectors.toSet())
                .subscribe(bookSet -> bookSet
                        .stream()
                        .forEach(data ->
                                log.info("{} : {}", data.getKorName(), data.getPrice())));

                Thread.sleep(12000);
    }

    private static Flux<Coffee> getCoffees() {
        final int[] count = {0};
        return Flux
                .fromIterable(SampleData.coffeeList)
                .delayElements(Duration.ofMillis(500))
                .map(coffee -> {
                    try {
                        count[0]++;
                        if (count[0] == 3) {
                            Thread.sleep(2000);
                        }
                    } catch (InterruptedException e) {
                    }
                    return coffee;
                })
                .timeout(Duration.ofSeconds(2))
                .retry(1)
                .doOnNext(coffee -> log.info("# getCoffees -> doOnNext: {}, {}",
                        coffee.getKorName(), coffee.getPrice()));
    }
}
```

- `delayElemnt()` Operator : 입력으로 주어진 시간만큼 각각의 데이터 emit을 지연시키는 Operator
- 세번째 데이터는 의도적으로 2초 더 지연시켰다.
- `timeout()` Operatoin에서 2초 안에 데이터가 emit되지 않으면 onError Signal 이벤트가 발생하도록 지정했기 때문에 2.5초가 지연되어 세 번째 데이터가 Downstream으로 emit되지 않았다.
- 원래라면 onError Signal 이벤트가 발생했기 때문에 모든 Sequence가 종료되어야하지만 `retry()` Operator를 추가했기 때문에 1회 재구독을 해서 Sequence를 다시 시작한다.
- 이제 timeout이 발생할 이유가 없으므로 데이터는 정상적으로 emit된다.

```java
20:07:24.620 [parallel-2] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 아메리카노, 2500
20:07:25.130 [parallel-4] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 카페라떼, 3500
20:07:27.638 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 아메리카노, 2500
20:07:27.639 [parallel-6] DEBUG reactor.core.publisher.Operators - onNextDropped: com.codestates.example.entity.Coffee@1831d4e
20:07:28.143 [parallel-2] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 카페라떼, 3500
20:07:28.647 [parallel-4] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 바닐라 라떼, 4500
20:07:29.151 [parallel-6] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 카라멜 마끼아또, 5500
20:07:29.657 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - # getCoffees -> doOnNext: 에스프레소, 5000
20:07:29.658 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - 아메리카노 : 2500
20:07:29.659 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - 카라멜 마끼아또 : 5500
20:07:29.659 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - 카페라떼 : 3500
20:07:29.659 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - 바닐라 라떼 : 4500
20:07:29.659 [parallel-8] INFO com.codestates.example.operators.create.TimeoutRetryExample01 - 에스프레소 : 5000
```