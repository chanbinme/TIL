* [목차](#목차)
* [Hamcrest](#hamcrest)
    + [Hamcrest란?](#hamcrest란)
    + [Hamcrest를 사용하는 이유](#hamcrest를-사용하는-이유)
    + [JUnit Assertion을 사용한 단위 테스트에 Hamcrest Assertion 적용해보기](#junit-assertion을-사용한-단위-테스트에-hamcrest-assertion-적용해보기)
        + [JUnit → Hamcrest 예 1](#junit-→-hamcrest-예-1)
        + [JUnit → Hamcrest 예 2](#junit-→-hamcrest-예-2)
        + [JUnit → Hamcrest 예 3](#junit-→-hamcrest-예-3)
        + [JUnit → Hamcrest 예 4](#junit-→-hamcrest-예-4)
    + [핵심 포인트](#핵심-포인트)



# Hamcrest

## Hamcrest란?

> Hamcrest는 JUnit 기반의 단위 테스트에서 사용할 수 있는 Assertion Framework이다.
> 
- JUnit에서도 Assertion을 위한 다양한 메서드를 지원하지만 Hamcrest만의 장점 때문에 JUnit에서 지원하는 Assertion 메서드보다 많이 사용된다.

## Hamcrest를 사용하는 이유

- Assertion을 위한 매쳐(Matcher)가 자연스러운 문장으로 이어지므로 가독성이 향상된다.
- 테스트 실패 메시지를 이해하기 쉽다.
- 다양한 Matcher를 제공한다.

## JUnit Assertion을 사용한 단위 테스트에 Hamcrest Assertion 적용해보기

## ****JUnit → Hamcrest 예 1****

### Junit에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class HelloJUnitTest {
    @DisplayName("Hello JUnit Test") // 실행 결과 창에 표시되는 이름을 지정
    @Test
    public void assertionTest() {
        String expected = "Hello, World";
        String actual = "Hello, JUnit";

        assertEquals(expected, actual); // expectedd와 실제 결과값(actual)이 일치하는지를 검증
    }
}
```

### Hamcrest에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;
import static org.hamcrest.core.IsEqual.equalTo;

public class HelloHamcrestTest {
    @DisplayName("Hello JUnit Test using Hacrest")
    @Test
    public void assertionTest1() {
        String expected = "Hello, JUnit";
        String actual = "Hello, World";

        /**
         * assert that actual is equal to exepected라는 하나의 문장으로 읽혀진다.
         * 첫 번째 파라미터는 테스트 대상의 실제 결과 값이다.
         * 두 번째 파라미터는 기대하는 값이다. 즉, 이런 값일거라고 기대하는 값이다.
         */
        assertThat(actual, is(equalTo(expected)));
    }
}
```

## ****JUnit → Hamcrest 예 2****

### JUnit에서의 failed 결과창

```java
expected: <Hello, World> but was: <Hello, JUnit>
Expected :Hello, World
Actual   :Hello, JUnit
```

### Hamcrest에서의 failed 결과창

```java
Expected: is "Hello, World"
     but: was "Hello, JUnit"
```

- Hamcrest의 Match를 사용해서 사람이 읽기 편한 자연스러운 Assertion 문장을 구성할 수 있으며, 실행 결과가 “failed”일 경우 자연스러운 “failed” 메시지를 확인할 수 있기 때문에 가독성이 높아진다.

## ****JUnit → Hamcrest 예 3****

```java
package com.codestates;

import java.util.HashMap;
import java.util.Map;

public class CryptoCurrency {
    public static Map<String, String> map = new HashMap<>();

    static {
        map.put("BTC", "Bitcoin");
        map.put("ETH", "Ethereum");
        map.put("ADA", "ADA");
        map.put("POT", "Polkadot");
    }
}
```

### JUnit에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertNotNull;

public class AssertionNotNullTest {

    @DisplayName("AssertionNull() Test")
    @Test
    public void assertNotNullTest() {
        String currencyName = getCryptoCurrency("ETH");

        assertNotNull(currencyName, "should be not null");
    }

    private String getCryptoCurrency(String unit) {
        return CryptoCurrency.map.get(unit);
    }
}
```

### Hamcrest에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.is;
import static org.hamcrest.core.IsNull.notNullValue;
import static org.hamcrest.core.IsNull.nullValue;

public class AssertionNullHamcrestTest {

    @DisplayName("AssertionNull() Test")
    @Test
    public void assertNotNullTest() {
        String currencyName = getCryptoCurrency("ETH");

     // currencyName is not Null value. 와 같이 가독성 좋은 하나의 문장이 되는 것을 볼 수 있다.
        assertThat(currencyName, is(notNullValue()));
//        assertThat(currencyName, is(nullValue()));

    }

    private String getCryptoCurrency(String unit) {
        return CryptoCurrency.map.get(unit);
    }
}
```

- 주석을 지우면

```java
Expected: is null
     but: was "Ethereum"
```

- Expected is null, but was “Ethereum” 같이 자연스러운 문장을 확인할 수 있다.

## ****JUnit → Hamcrest 예 4****

### JUnit에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertThrows;

public class AssertionExceptionTest {

    @DisplayName("throws NullPointException when map.get()")
    @Test
    public void assertionThrowExceptionTest() {
        // 첫 번째 파라미터에는 발생이 기대되는 예외 클래스를 입력하고, 두 번째 파라미터인 람다 표현식에서는 테스트 대상 메서드를 호출
        assertThrows(NullPointerException.class, () -> getCryptoCurrency("XRP"));
    }

    private String getCryptoCurrency(String unit) {
        return CryptoCurrency.map.get(unit).toUpperCase();
    }
}
```

### Hamcrest에서의 Assertion

```java
package com.codestates.basic;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.equalTo;
import static org.hamcrest.Matchers.is;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class AssertionExceptionHamcrestTest {

    @DisplayName("throws NullPointerException when map.get()")
    @Test
    public void assertionThrowExceptionTest() {
        // Hamcrest만으로 Assertion을 구성하기 힘들기 때문에 assertThrows() 메서드를 이용해서
        // assertThrows()의 리턴값으로 전달 받은 Exception 내부의 정보를 가져와서
        Throwable actualException = assertThrows(NullPointerException.class,
                () -> getCrypCurrency("XRP"));

        // assertThat(expectedException.getCause(), is(equalTo(null))); 처럼 추가로 검증을 했다.
        assertThat(actualException.getCause(), is(equalTo(null)));
    }

    private String getCrypCurrency(String unit) {
        return CryptoCurrency.map.get(unit).toUpperCase();
    }
}
```

- 1차적으로 `assertThrows`에서 NPE가 발생하므로 Assertion 결과는 passed이고 결과 값인 `actualexception.getCause()` 가 `null` 이므로 해당 Assertion 결과 역시 passed이다.

## 핵심 포인트

- Hamcrest는 JUnit 기반의 단위 테스트에서 사용할 수 있는 Assertion Framework이다.
- Hamcrest는 다음과 같은 이유로 JUnit에 지원하는 Assertion 메서드보다 많이 사용된다.
    - Assertion을 위한 매쳐(Matcher)가 자연스러운 문장으로 이어지므로 가독성이 향상 된다.
    - 테스트 실패 메시지를 이해하기 쉽다.
    - 다양한 Matcher를 제공한다.
- Hamcrest만으로 던져진(thrown) 예외를 테스트하기 위해서는 Custom Matcher를 직접 구현해서 사용할 수 있다.