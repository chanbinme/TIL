# 목차
* [목차](#목차)
* [마블 다이어그램(Marble Diagram)](#마블-다이어그램marble-diagram)
    + [마블 다이어그램이란?](#마블-다이어그램이란)
    + [마블 다이어그램으로 Mono와 Flux 이해하기](#마블-다이어그램으로-mono와-flux-이해하기)
        + [Mono의 마블 다이어그램](#mono의-마블-다이어그램)
        + [Flux의 마블 다이어그램](#flux의-마블-다이어그램)
        + [Operator의 마블 다이어그램](#operator의-마블-다이어그램)
    + [핵심](#핵심)

# 마블 다이어그램(Marble Diagram)

## 마블 다이어그램이란?

> 다이어그램 상에서 시간의 흐름에 따라 변화하는 데이터의 흐름
> 
- 리액티브 프로그래밍에서는 어떤 Operator를 사용하느냐에 따라서 데이터의 흐름이 다양하게 변화할 수 있으며, 이러한 복잡한 데이터의 흐름을 마블 다이어그램을 통해 좀 더 쉽게 이해할 수 있다.

## 마블 다이어그램으로 Mono와 Flux 이해하기

### Mono의 마블 다이어그램

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/95dd50a0-39b8-4832-a273-9448cc895de5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221130T112003Z&X-Amz-Expires=86400&X-Amz-Signature=05734d3a438821a71794b72b06cc58ae4c244be166da43f40d9b9ff45a3c08fb&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 그림은 Reactor의 데이터 타입 중 하나인 Mono를 마블 다이어그램으로 표현한 것이다.
- Mono의 마블 다이어그램을 보면서 마블 다이어그램을 이해하는 방법을 확인해보자
- 마블 다이어그램에는 아래 위로 두 개의 타임 라인이 있는데 모두 데이터가 흘러가는 시간의 흐름을 표현하고 있다. 시간은 왼쪽에서 오른쪽으로 흘러가기 때문에 시간 상으로는 왼쪽이 빠른시간이다.
    1. 원본 Mono(Original Mono)에서 Sequence가 시작되는 것을 타임라인으로 표현한 것이다.
    2. Mono의 Sequence에서 데이터가 emit되는 것을 표현했다.  그림 상에서 구슬 모양의 데이터 하나가 emit되는 것을 확인할 수 있는데, 단순히 그냥 구슬 모양의 데이터하나만 표시한게 아니라 Mono는 0건 또는 1건의 데이터만 emit하는 Reactor 타입이기때문에 마블 다이어그램에서 이를 표현하고 있는 것이다. 
    3. 수직 막대 바는 Mono의 Sequence가 정상 종료됨을 의미한다.
    4. Mono에서 지워하는 어떤 Operator에서 입력으로 들어오는 구슬 모양의 데이터를 가공처리 하는 것을 표현하고 있다.
    5. Operator에서 가공 처리된 데이터가 DownStream으로 전달될 때의 타임라인이다. 
    6. 만약 Mono에서 emit 된 데이터가 처리되는 과정에 에러가 발생한다면 X로 표시한다.
    - `|` : 정상종료
    - `X` : 비정상 종료

### Flux의 마블 다이어그램

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/38b8d70a-83bd-47dc-9986-7264c0f3d14c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221130T111926Z&X-Amz-Expires=86400&X-Amz-Signature=0fd262de0fbec0311eff414aa1526fa776f80bae7bc5a11bbc2feb902d7ce030&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Flux의 마블 다이어그램은 Mono의 마블 다이어그램과 큰 차이가 없다.
- emit 되는 데이터가 Mono와 달리 구슬이 하나가 아니라 여러 개의 구슬이 보인다. 이는 Mono가 0 또는 1개의 데이터만 emit하는 것과 달리 Flux는 여러 개(0…N)의 데이터를 emit하는 Reactor 타입임을 표현한 것이다.

### Operator의 마블 다이어그램

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fa67c352-58c3-487a-af75-d74d70d1153d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221130T111936Z&X-Amz-Expires=86400&X-Amz-Signature=bf1739327a7d729b63657e4354dc5b802dfc0cdac038c76f6f2a50480a6bac69&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- Reactor에서 마블 다이어그램을 보는 제일 중요한 이유는 Reactor에서 제공하는 수많은 Operator의 내부 동작을 조금 더 쉽게 이해하고, 해당 Operator를 적절하게 사용하기 위함이다.
- map() Operator는 입력으로 들어오는 데이터를 개발자가 구현하는 동작대로 변환해서 Downstream으로 전달하는 역할을 한다.

```java
import reactor.core.publisher.Flux;

public class MarbleDiagramExample {
    public static void main(String[] args) {
        Flux
            .just("Green-Circle", "Orange-Circle", "Blue-Circle")   // (1)
            .map(figure -> figure.replace("Circle", "Rectangle"))   // (2)
            .subscribe(System.out::println);   // (3)
    }
}
```

1. 세 개의 문자열을 emit한다.
2. map() Operator 내부에서 동그라미를 네모로 바꾸고 있다.
3. map() Operator 내부에서 변환된 문자열을 출력한다.

## 핵심

- 마블 다이어그램은 굉장히 중요하다. Reactor의 공식 API 문서에서 마블 다이어그램 없이 문장으로 된 API 설명을 읽어도 이해되지 않는 부분은 마블 다이어그램을 보면 대부분 이해가 된다.
    
    마블 다이어그램만으로 이해가 안될 때는 해당 Operator를 먼저 이해하려고 노력해 본 다음에 이해가 되지 않을 경우 API 설명을 읽어보는 것이 좋다.
    
- 마블 다이어그램(Marble Diagram) 상에서 구슬 모양의 알록달록한 동그라미는 하나의 데이터를 의미하며, 시간의 흐름에 따라 변화하는 데이터의 흐름을 표현한다.
- 마블 다이어그램은 Reactor에서 제공하는 수많은 Operator의 내부 동작을 조금 더 쉽게 이해하고, 해당 Operator를 적절하게 사용하는데 도움을 준다.
- Mono는 0건 또는 1건의 데이터만 emit하는 Reactor 타입이다.
- Flux는 여러 개(0…N)의 데이터를 emit하는 Reacotr 타입이다.