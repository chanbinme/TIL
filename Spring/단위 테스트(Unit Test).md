* [목차](#목차)
* [단위 테스트(Unit Test)](#단위-테스트unit-test)
    + [단위 테스트란?](#단위-테스트란)
        + [기능 테스트](#기능-테스트)
        + [통합 테스트](#통합-테스트)
        + [슬라이스 테스트](#슬라이스-테스트)
        + [단위 테스트](#단위-테스트)
    + [단위 테스트를 해야 하는 이유](#단위-테스트를-해야-하는-이유)
        + [테스트 케이스(Test Case)란?](#테스트-케이스test-case란)
    + [단위 테스트를 위한 F.I.R.S.T 원칙](#단위-테스트를-위한-first-원칙)  
    + [Given-When-Then 표현 스타일](#given-when-then-표현-스타일) 
        + [Assertion(어써션)이란?](#assertion어써션이란)
    + [JUnit 없이 단위 테스트 적용해보기](#junit-없이-단위-테스트-적용해보기)
    + [핵심 포인트](#핵심-포인트)

# 단위 테스트(Unit Test)

## 단위 테스트란?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/75116c34-2caa-446e-b96f-ff10860ceaf9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221108T123217Z&X-Amz-Expires=86400&X-Amz-Signature=0f6aae5e71b8e8660e6c08884f60bb633ecb345e1220b7f6585f7262dce27410&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 커피 주문 샘플 애플리케이션을 단위라는 기준을 적용해서 표현한 그림이다.

### 기능 테스트

- 기능 테스트가 테스트의 범위가 제일 큰 것을 볼 수 있다.
- **기능 테스트는 주로 애플리케이션을 사용하는 사용자 입장에서 애플리케이션이 제공하는 기능이 올바르게 동작하는지를 테스트**한다.
- 기능 테스트를 하는 주체는 일반적으로 테스트 전문 부서(QA 부서) 또는 외부 QA 업체가 된다.
- 기능 테스트의 경우 API 툴이나 데이터베이스까지 연관되어 있어서 HTTP 통신도 해야되고, 데이터베이스 연결도 해야되는 등 **애플리케이션과 연관된 대상이 많기 때문에 흔히 이야기하는 단위 테스트로 부르기는 힘들다.**

### 통합 테스트

- 기능 테스트와는 달리 통합 테스트를 하는 주체는 일반적으로 애플리케이션을 만든 개발자 또는 개발팀이다.
- 통합 테스트는 클라이언트 측 툴 없이 개발자가 짜 놓은 테스트 코드를 실행시켜서 이루어지는 경우가 많다.
- 예를 들어, 개발자가 Controller의 API를 호출하는 테스트 코드를 작성한 후 실행하면 서비스 계층과 데이터 액세스 계층을 거쳐 DB에 실제로 접속해서 기대했던 대로 동작을 하는지 테스트하는 것은 통합 테스트의 하나라고 볼 수 있다.
- 하지만 통합 테스트 역시 애플리케이션의 여러 계층이 연관되어 있으며, DB까지 연견될어 있어서 **독립적인 테스트가 가능하다고 볼 수는 없기 때문에 단위 테스트라고 하기에는 범위가 여전히 크다.**

### 슬라이스 테스트

- 슬라이스 테스트는 애플리케이션을 특정 계층으로 쪼개어서 하는 테스트를 말한다.
- API 계층, 서비스 계층, 데이터 액세스 계층이 각각 슬라이스 테스트의 대상이 될 수 있다.
- 하지만 슬라이스 테스트 역시 해당 계층에서 HTTP 요청이 필요하고, 외부 서비스 연동, DB와 연동되어 있기 때문에 **슬라이스 테스트는 단위 테스트라고 하기에는 어렵다.**

### 단위 테스트

- 서비스 계층의 경우, 애플리케이션의 핵심 로직인 비즈니스 로직을 구현하는 계층이다.
- 일반적으로 직접 구현하는 핵심 로직. 즉, **비즈니스 로직에서 사용하는 클래스들이 독립적으로 테스트하기 가장 좋은 대상**이기 때문에 단위 테스트라고 하는 경우가 많다.
- 주로 메서드를 많이 테스트한다. **단위 테스트 코드는 메서드 단위로 대부분 작성**된다고 생각하면 좋다.
- 일반적으로 단위 테스트는 최대한 독립적이고 최대한 작은 단위인 것이 더 좋다. 더 작은 단위일수록 다른 연관된 기능들을 생각할 필요도 없고, 테스트 코드를 짜기도 더 단순해지기 때문에 그만큼 빠르게 테스트를 수행할 수 있다.
- DB를 사용한다고 하더라도 데이터베이스의 상태가 테스트 이전과 이후가 동일하게 유지될 수 있다면 **데이터베이스가 연동된다고 하더라도 단위 테스트에 포함될 수 있다.**

## 단위 테스트를 해야 하는 이유

- 테스트를 하기위해 IDE를 실행시키고, Postman을 열어서 HTTP 요청을 보내는 번거로운 일들을 단순화 할 수 있다.
- 내가 구현한 코드가 의도한 대로 동작하는지 그 결과를 빠르게 확인할 수 있다.
- 작은 단위의 테스트로 미리 버그를 찾을 수 있기 때문에 애플리케이션의 덩치가 커진 상태에서 문제의 원인을 찾아내는 것보다 상대적으로 더 적은 시간 안에 문제를 찾아낼 가능성이 높다.
- 테스트 케이스가 잘 짜여져 있으면 버그가 발생하더라도 심리적인 안정감이 좀 더 높아진다.

### 테스트 케이스(Test Case)란?

> 테스트 케이스란 테스트를 위한 입력 데이터, 실행 조건, 기대 결과를 표현하기 위한 명세를 의미한다. 즉, 메서드 등 하나의 단위를 테스트하기 위해 작성하는 테스트 코드라고 생각하면 좋다.
> 

## 단위 테스트를 위한 F.I.R.S.T 원칙

### Fast(빠르게)

- 일반적으로 작성한 테스트 케이스는 빨라야 한다.
- 자주 돌려야 문제를 빨리 찾을 수 있는데, 너무 느려서 돌리기 힘들다면 테스트 케이스를 작성하는 의미가 퇴색될 것이다.

### Independent(독립적으로)

- 각각의 테스트 케이스는 독립적이어야 한다.
- 일반적으로 테스트 케이스를 작성할 때, 클래스 단위로 해당 클래스 내의 메서드 동작으로 테스트한다. 클래스 안에 메서드가 여러개 존재할 경우가 많을테니 테스트 클래스 안에 테스트 케이스도 하나 이상이 될 것이다.
- 이 때, 어떤 테스트 케이스를 먼저 실행시켜도 순서와 상관없이 정상적인 실행이 보장되어야 한다.
- 예를 들어, A 테스트 케이스를 먼저 실행시킨 후에 B 테스트 케이스를 실행시켰더니 테스트에 실패한다면 해당 테스트 케이스끼리 독립적이지 않은 것이다.

### Self-validating(셀프 검증이 되도록)

- 단위 테스트는 성공 또는 실패라는 자체 검증 결과를 보여주어야 한다.
- 테스트 케이스 스스로 결과가 옳은지 그른직 판단할 수 있어야 한다는 의미이다.

### Timely(시기 적절하게)

- 단위 테스트는 테스트 하려는 기능 구현을 하기 직전에 작성해야 한다.
- TDD(테스트 주도 개발) 개발 방식에는 기능 구현 전에 실패하는 테스트 케이스를 먼저 작성하는 방식을 취하지만 실제로 기능 구현도 하지 않았는데 테스트 케이스부터 먼저 작성한다는게 쉽지 않다.
- 다만, 기능 구현을 먼저 하더라도 너무 많은 구현 코드가 작성된 상태에서 테스트 케이스를 작성하려면 오히려 테스트 케이스를 작성하는데 더 많은 시간을 들일 가능성이 있다.
- **구현하고자 하는 기능을 단계적으로 조금씩 업그레이드하면서 그 때, 그 때 테스트 케이스 역시 단계적으로 업그레이드 하는 방식이 더 낫다.**

## Given-When-Then 표현 스타일

- BDD(Behavior Driven Development) 테스트 방식에서 사용하는 용어다.
- 테스트 케이스의 가독성을 높이기 위해 사용하는 유용한 방법이다.
- `Given`
    - 테스트를 위한 준비 과정을 명시할 수 있다.
    - 테스트에 필요한 전제 조건들이 포함된다.
    - 테스트 대상에 전달되는 입력값(테스트 데이터) 역시 Given에 포함된다.
- `When`
    - 테스트 할 동작(대상)을 지정한다.
    - 단위 테스트에서는 일반적으로 메서드 호출을 통해 테스트를 진행하므로 한두줄 정도로 작성이 끝나는 부분이다.
- `Then`
    - 테스트의 결과를 검증하는 영역이다.
    - 일반적으로 예상하는 값(expected)과 테스트 대상 메서드의 동작 수행 결과(actual) 값을 비교해서 기대한대로 동작을 수행하는지 검증(Assertion)하는 코드들이 포함된다.

### Assertion(어써션)이란?

- 단언, 단정이라는 의미로 테스트 세계에서는 테스트 결과를 검증할 때 주로 사용한다.
- 테스트 케이스의 결과가 반드시 참(true)이어야 한다는 것을 논리적으로 표현한 것이 Assertion인데, 한마디로 **예상하는 결과 값이 참(true)이길 바라는 것**이라고 이해하면 좋다.

## JUnit 없이 단위 테스트 적용해보기

```java
public class StampCalculator {

    public static int calculateStampCount(int nowCount, int earned) {
        return nowCount + earned;
    }

    public static int calculateEarnedStampCount(Order order) {
        return order.getOrderCoffees().stream()
                .map(orderCoffee -> orderCoffee.getQuantity())
                .mapToInt(quantity -> quantity)
                .sum();
    }
}
```

```java
public class StampCalculatorTestWithoutJUnit {
    public static void main(String[] args) {
        calculateStampCountTest();        
        calculateEarnedStampCountTest();  
    }

    private static void calculateStampCountTest() {
        // given
        int nowCount = 5;
        int earned = 3;
        

        // when
        int actual = StampCalculator.calculateStampCount(nowCount, earned);

        int expected = 7;

        // then
        System.out.println(expected == actual);
    }

    private static void calculateEarnedStampCountTest() {
        // given
        Order order = new Order();
        OrderCoffee orderCoffee1 = new OrderCoffee();
        orderCoffee1.setQuantity(3);

        OrderCoffee orderCoffee2 = new OrderCoffee();
        orderCoffee2.setQuantity(5);

        order.setOrderCoffees(List.of(orderCoffee1, orderCoffee2));

        int expected = orderCoffee1.getQuantity() + orderCoffee2.getQuantity();

        // when
        int actual = StampCalculator.calculateEarnedStampCount(order);

        // then
        System.out.println(expected == actual);
    }
}
```

```java
false
true
```

## 핵심 포인트

- 테스트란 어떤 대상에 대한 일정 기준을 정해놓고, 그 대상이 정해진 기준에 부합하는지를 검증하는 과정이다.
- 기능 테스트는 주로 애플리케이션을 사용하는 사용자 입장에서 애플리케이션이 제공하는 기능이 올바르게 작동하는지 테스트하는 것을 의미한다.
- 통합 테스트는 클라이언트 측 툴 없이 개발자가 짜 놓은 테스트 코드를 실행시켜서 이루어지는 경우가 많다.
- 슬라이스 테스트는 애플리케이션을 특정 계층으로 쪼개어서 하는 테스트를 의미한다.
- 단위 테스트는 일반적으로 메서드 단위로 작성한다.
- 테스트 케이스란 테스트를 위한 입력 데이터, 실행 조건, 기대 결과를 표현하기 위한 명세를 의미한다.
- 단위 테스트를 위한 F.I.R.S.T 원칙
    - Fast(빠르게)
    - Indipendent(독립적으로)
    - Repeatable(반복 가능하도록)
    - Self-validating(셀프 검증이 되도록)
    - Timely(시기 적절하게)
- Give-When-Then 표현 스타일
    - `Given` : 테스트를 위한 준비 과정을 명시한다.
    - `When` : 테스트 할 동작(대상)을 지정한다.
    - `Then` : 테스트의 결과를 검증(Assertion)한다.
- Assertion(어써션)은 예상하는 결과 값이 참(true)이길 바라는 것을 의미한다.