# 목차
* [목차](#목차)
* [PSA(Portable Service Absraction)](#psaportable-service-absraction)
    + [PSA(Portable Service Abstraction)란?](#psaportable-service-abstraction란)
        + [추상화의 개념](#추상화abstraction의-개념)
        + [예제](#예제)
        + [추상화를 하는 이유](#추상화를-하는-이유)
    + [서비스에 적용하는 일관된 서비스 추상화(PSA) 기법](#서비스에-적용하는-일관된-서비스-추상화psa-기법)
    + [PSA가 필요한 이유](#psa가-필요한-이유)
    + [핵심 포인트](#핵심-포인트)
    + [직접 추상화 구현해보기!](#직접-추상화-구현해보기)

# PSA(Portable Service Absraction)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/418ae95a-44e5-4d0d-a864-d6cf72869ddd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T114051Z&X-Amz-Expires=86400&X-Amz-Signature=8080a486ddd2367a1f1caff8dbe19dfa65e35889b121508096521e2739a0363e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## PSA(Portable Service Abstraction)란?

### 추상화(Abstraction)의 개념

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f5ae5a7f-752e-498d-96ad-103d3c8a82cc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T114102Z&X-Amz-Expires=86400&X-Amz-Signature=dc3d05f69e78e55cef7d0b6a9a42cce570c39c8f51e91d4b52b10514ab316e1c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

프로그래밍에서의 추상화란 **어떤 클래스의 본질적인 특성만을 추출해서 일반화하는 것**을 말한다.

Java에서 코드로 추상화를 표현할 수 있는 대표적인 방법은 바로 추상 클래스와 인터페이스이다. 

### 예제

미취학 아동을 관리하는 애플리케이션을 설계하면서 **아이 클래스를 일반화(추상화)**한다라고 가정해보자.

아기의 속성과 동작을 일반화해서 멤버 변수와 메서드로 표현해보자. 먼저 아이를 관리하는 관점에서 아이의 일반적인 속성으로는 **이름, 키, 몸무게, 혈액형, 나이** 등이 있다. 그리고 일반적으로 아이가 할 수 있는 동작으로는 **웃다, 울다, 자다, 먹다** 등이 있다.

```java
public abstract class Child {
	protected String childType;
	protected double height;
	protected double weight;
	protected String bloodType;
	protected int age;

	protected abstract void smile();
	protected abstract void cry();
	protected abstract void sleep();
	protected abstract void eat();
```

### 추상화를 하는 이유

추상화를 하는 이유를 코드를 통해 알아보자

```java
// NewBornBaby.java(신생아)
public class NewBornBaby extends Child {
    @Override
    protected void smile() {
        System.out.println("신생아는 가끔 웃어요");
    }

    @Override
    protected void cry() {
        System.out.println("신생아는 자주 울어요");
    }

    @Override
    protected void sleep() {
        System.out.println("신생아는 거의 하루 종일 자요");
    }

    @Override
    protected void eat() {
        System.out.println("신생아는 분유만 먹어요");
    }
}

// Infant.java(2개월 ~ 1살)
public class Infant extends Child {
    @Override
    protected void smile() {
        System.out.println("영아는 많이 웃어요");
    }

    @Override
    protected void cry() {
        System.out.println("영아는 종종 울어요");
    }

    @Override
    protected void sleep() {
        System.out.println("영아부터는 밤에 잠을 자기 시작해요");
    }

    @Override
    protected void eat() {
        System.out.println("영아부터는 이유식을 시작해요");
    }
}

// Toddler.java(1살 ~ 4살)
public class Toddler extends Child {
    @Override
    protected void smile() {
        System.out.println("유아는 웃길 때 웃어요");
    }

    @Override
    protected void cry() {
        System.out.println("유아는 화가나면 울어요");
    }

    @Override
    protected void sleep() {
        System.out.println("유아는 낮잠을 건너뛰고 밤잠만 자요");
    }

    @Override
    protected void eat() {
        System.out.println("유아는 딱딱한 걸 먹기 시작해요");
    }
}
```

위 코드들은 Child 클래스의 추상화된 동작을 자신만의 고유한 동작으로 재구성하고 있다. 위 코드들을 사용하기 위해서 어떤 방식으로 접근하면 될까?

```java
public class ChildManageApllication {
	public static void main(String[] args) {
		Child newBornBaby = new newBornBaby();
		Child infant = new Infant();
		Child toddler = new Toddler();

		newBornBaby.sleep();
		infant.sleep();
		toddler.sleep();
	}
}

// 결과
신생아는 거의 하루 종일 자요
영아부터는 밤에 잠을 자기 시작해요
유아는 낮잠을 건너뛰고 밤잠만 자요
```

Child라는 상위 클래스에 일반화 시켜 놓은 아이의 동작을 각 클래스로 연력별 아이의 동작으로 구체화시켜서 사용하고 있다.

여기서 중요한 것은 클라이언트는 `NewBornBaby`, `Infant`, `Toddler` 를 사용할 때 구체화 클래스의 객체를 자신의 타입에 할당하지 않고 Child 클래스 변수에 할당해서 접근한다.

이렇게 되면 클라이언트 입장에서는 Child라는 추상 클래스만 **일관되게 바라보며 하위 클래스의 기능을 사용할 수 있다.**

이처럼 클라이언트가 **추상화 된 상위 클래스를 일관되게 바라보며 하위 클래스의 기능을 사용하는 것이 바로 일관된 서비스 추상화(PSA)의 기본 개념**이다.

## 서비스에 적용하는 일관된 서비스 추상화(PSA) 기법

서비스 추상화란 위와 같은 추상화의 개념을 애플리케이션에 사용하는 서비스에 적용하는 기법이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e527cc2a-391f-4a6e-9f5c-25e29f66f9ba/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T114118Z&X-Amz-Expires=86400&X-Amz-Signature=2a1fe6946216d30b2bcf863a54c335c604ba6663ff840eee5870b9297f83dc95&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 그림은 Java 콘솔 애플리케이션에서 클라이언트가 데이터베이스에 연결하기 위해 JdbcConnect를 사용하기 위한 서비스 차상화의 예이다.

즉, **JdbcConnector가 애플리케이션에서 이용하는 하나의 서비스**가 되는 것이다.

`DbClient`는 `OracleJdbcConnector`, `MariaDBJdbcConnector`, `SQLiteJdbcConnector`같은 JdbcConnector 인터페이스의 구현체에 직접적으로 연결해서 Connection을 얻는 것이 아니라 **JdbcConnector 인터페이스를 통해 간접적으로 연결**되어(**느슨한 결합**) Connection 객체를 얻는 것을 볼 수 있다.

`DbClient` 에서 어떤 구현체를 사용하더라도 Connection을 얻는 방식은 getConnection() 메서드를 사용해야 하기 때문에 동일하다. 즉, **일관된 방식으로 해당 서비스의 기능을 사용할 수 있다는 의미이다.**

이처럼 애플리케이션에서 특정 서비스를 이용할 때, **서비스의 기능을 접근하는 방식 자체를 일관되게 유지하면서 기술 자체를 유연하게 사용할 수 있도록 하는 것을 PSA(일관된 서비스 추상화)**라고 한다.

```java
// DbClient.java
public class DbClient {
    public static void main(String[] args) {
        // Spring DI로 대체 가능
        JdbcConnector connector = new SQLiteJdbcConnector(); // (1)

        // Spring DI로 대체 가능
        DataProcessor processor = new DataProcessor(connector); // (2)
        processor.insert();
    }
}

// DataProcessor.java
public class DataProcessor {
    private Connection connection;

    public DataProcessor(JdbcConnector connector) {
        this.connection = connector.getConnection();
    }

    public void insert() {
        // 실제로는 connection 객체를 이용해서 데이터를 insert 할 수 있다.
        System.out.println("inserted data");
    }
}

// JdbcConnector.java
public interface JdbcConnector {
    Connection getConnection();
}

// MariaDBJdbcConnector.java
public class MariaDBJdbcConnector implements JdbcConnector {
    @Override
    public Connection getConnection() {
        return null;
    }
}

// OracleJdbcConnector.java
public class OracleJdbcConnector implements JdbcConnector {
    @Override
    public Connection getConnection() {
        return null;
    }
}

// SQLiteJdbcConnector.java
public class SQLiteJdbcConnector implements JdbcConnector {
    @Override
    public Connection getConnection() {
        return null;
    }
}
```

- (1)에서 `SQLiteJdbcConnecto`구현체의 객체를 생성해서 `JdbcConnector`인터페이스 타입의 변수에 할당(**업캐스팅**)하고 있는 것을 볼 수 있다.
- (2)에서 데이터를 데이터베이스에 저장하는 기능을 하는 DataProcessor 클래스의 생성자로 JdbcConnector 객체를 전달하고 있다.**(의존성 주입)**
- 만약 다른 애플리케이션에서 SQLite에서 Oracle 데이터베이스로 바꿔야 한다고 한다면, `JdbcConnecto` 서비스 모듈을 그대로 가져와서 (1)의 `new SQliteJdbcConnector()` 를 `new OracleJdbcConnector()` 로 바꿔서 사용하면 된다.

## PSA가 필요한 이유

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/959ea749-e9cc-406d-a72f-c5ec4e92c27b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T114542Z&X-Amz-Expires=86400&X-Amz-Signature=8dd7a37f1e3e6d34f5cccfb795a1daa841957d0c4fc5fece0ddff5018b3b9f8a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

PSA가 필요한 이유는 **어떤 서비스를 이용하기 위한 접근 방식을 일관된 방식으로 유지함으로써 애플리케이션에서 사용하는 기술이 변경되더라도 최소한의 변경만으로 변경된 요구 사항을 반영하기 위함**이다.

즉, PSA를 통해서 **애플리케이션의 요구 사항 변경에 유연하게 대처할 수 있다.**

Spring은 PSA를 적극적으로 지원하고 있다. 적용된 분야로는 트랜잭션 서비스, 메일 서비스, Spring Data 서비스 등이 있다.

## 핵심 포인트

- 객체지향 프로그래밍 세계에서 어떤 클래스의 본질적인 특성만을 추출해서 일반화하는 것을 추상화(Abstraction)라고 한다.
- 클라이언트가 **추상화 된 상위 클래스를 일관되게 바라보며 하위 클래스의 기능을 사용하는 것이 바로 일관된 서비스 추상화(PSA)의 기본 개념**이다.
- 애플리케이션에서 특정 서비스를 이용할 때, **서비스의 기능을 접근하는 방식 자체를 일관되게 유지하면서 기술 자체를 유연하게 사용할 수 있도록 하는 것을 PSA(일관된 서비스 추상화)**라고 한다.
- PSA가 필요한 주된 이유는 **어떤 서비스를 이용하기 위한 접근 방식을 일관된 방식으로 유지함으로써 애플리케이션에서 사용하는 기술이 변경되더라도 최소한의 변경만으로 요구 사항을 반영하기 위함**이다.

## 직접 추상화 구현해보기!
```java
package com.codestates.chapter2.psa;

public abstract class Chicken {
    protected String chickenType;
    protected double weight;
    protected String brandName;
    protected int price;

    protected abstract void grade();
    protected abstract void discription();
    protected abstract void taste();
}
```

```java
package com.codestates.chapter2.psa;

public class GoldOliveChicken extends Chicken {
    @Override
    protected void grade() {
        System.out.println("치킨 인기투표 2등을 차지했어요.");
    }

    @Override
    protected void discription() {
        System.out.println("BBQ의 대표메뉴. 줄여서 황올이라고 부릅니다. 맛 자체는 확실히 다른 후라이드 치킨과는 차별화된 맛을 자랑합니다. 블라인드 테스트에서 바로 맞출 수 있을정도입니다.");
    }

    @Override
    protected void taste() {
        System.out.println("매콤짭짤한 맛");
    }
}
```

```java
package com.codestates.chapter2.psa;

public class HoneyCombo extends Chicken {
    @Override
    protected void grade() {
        System.out.println("치킨 인기투표 3등을 차지했어요.");
    }

    @Override
    protected void discription() {
        System.out.println("교촌치킨의 대표메뉴. 교촌 오리지날에 꿀을 더한 단짠 스타일의 치킨입니다.");
    }

    @Override
    protected void taste() {
        System.out.println("달콤짭짤한 맛");
    }
}
```

```java
package com.codestates.chapter2.psa;

public class Purrinkle extends Chicken {
    @Override
    protected void grade() {
        System.out.println("치킨 인기투표 1등을 차지했어요.");
    }

    @Override
    protected void discription() {
        System.out.println("BHC의 역작. 뿌링 시즈닝을 뿌린 치킨에 에멘탈 치즈, 요거트 베이스의 뿌링뿌링 소스에 찍어 먹습니다.");
    }

    @Override
    protected void taste() {
        System.out.println("달콤짭짤한 맛");
    }
}
```

```java
package com.codestates.chapter2.psa;

public class ChickenManageApllication {
    public static void main(String[] args) {
        Chicken purrinkle = new Purrinkle();
        Chicken goldOliveChicken = new GoldOliveChicken();
        Chicken honeyCombo = new HoneyCombo();

        purrinkle.discription();
        goldOliveChicken.discription();
        honeyCombo.discription();
    }
}
```