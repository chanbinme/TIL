* [목차](#목차)
* [DI(Dependency Injection)](#didependency-injection)
    + [DI(Dependency Injection)란?](#didependency-injection란)
    + [What(의존성 주입은 무엇일까?)](#what의존성-주입은-무엇일까)
        + [클래스 간의 의존 관계 성립](#클래스-간의-의존-관계-성립)
        + [의존성을 주입해보자](#의존성을-주입해보자)
    + [Why(의존성 주입은 왜 필요할까?)](#why의존성-주입은-왜-필요할까)
    + [How(느슨한 의존성 주입은 어떻게 할까?)](#how느슨한-의존성-주입은-어떻게-할까)
    + [Who(Spring에서 의존성 주입을 누가 해줄까?)](#whospring에서-의존성-주입을-누가-해줄까)
    + [핵심 포인트](#핵심-포인트)

        # DI(Dependency Injection)

## DI(Dependency Injection)란?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d073a9c5-fcbb-457e-920e-1698e0033443/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115254Z&X-Amz-Expires=86400&X-Amz-Signature=dc4efa41876c82011317162be8919808b8b92d122d78e15daf92b2b2489d7248&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

IoC(제어의 역전)는 서버 컨테이너 기술, 디자인 패턴, 객체 지향 설계 등에 적용하게 되는 일반적인 개념이라면 DI(Dependency Injection)는 IoC 개념을 조금 구체화시킨 것이라고 볼 수 있다.

Dependency는 **의존하는 또는 종속되는** 이라는 의미를 가지고 있다. 그리고 Injection은 **주입**이라는 의미를 가지고 있다. 

단순히 의존성 주입이라고 하면 이해하기 어렵다. 육하원칙에 따라서 단계적으로 알아보자

## What(의존성 주입은 무엇일까?)

- 객체지향 프로그래밍에서 의존성이라고 하면 대부분 객체 간의 의존성을 의미한다. 그런데 객체 간의 의존성이라는 말이 도대체 무슨 뜻일까?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/24afd2c6-3b4b-4d60-bdee-faa7158ed01b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115305Z&X-Amz-Expires=86400&X-Amz-Signature=d9134ddedb2ffee0a8c326fd19b624c2c895d3519f95de86c05145e1df50852f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

A, B라는 두 개의 클래스 파일을 만들어서 A클래스에서 B클래스의 기능을 사용하기 위해 B클래스에 구현되어 있는 메서드를 호출한다고 생각해보자

이처럼 **A클래스가 B클래스의 기능을 사용할 때, A클래스는 B클래스에 의존한다**고 한다. 쉽게 말해 A클래스의 프로그래밍 로직 완성을 위해 B클래스에게 도움을 요청한다고 보면 이해하기 쉽다.

> 클래스 다이어그램은 클래스들의 관계를 다이어그램으로 표현한 것인데, 애플리케이션 설계를 위해서 굉장히 많이 사용되는 다이어그램이다. 일반적으로 애플리케이션 구현에 클래스들의 큰 그림을 그려보기 위해서 사용한다. 

클래스 다이어그램은 애플리케이션 설계에 있어 굉장히 중요한 역할을 하기 때문에 시간 날 때마다 틈틈이 클래스 다이어그램 작성을 연습하는 것이 좋다.
(클래스 다이어그램 도구: [Visual Paradigm](https://online.visual-paradigm.com/diagrams/features/))
> 

코드를 통해 의존성 주입이 무엇인지 알아보자

### 클래스 간의 의존 관계 성립

```java
public class MenuController {
	public static void main(String[] args) {
		MenuService menuService = new MenuService();
		List<Menu> menuList = menuService.getMenuList();
	}
}
```

```java
public class MenuService {
	public List<Menu> getMenuList() {
		return null;
	}
}
```

`MenuController` 클래스는 클라이언트의 요청을 받는 엔드포인트(Endpoint) 역할을 하고, `MenuService` 클래스는 `MenuController` 클래스가 전달 받은 클라이언트의 요청을 처리하는 역할을 한다.

> 엔드포인트란 클라이언트가 서버의 자원(Resource)을 이용하기 위한 끝 지점을 의미한다.
> 

`MenuController` 클래스는 메뉴판에 표시되는 메뉴 목록을 조회하기 위해서 `new` 키워드를 사용해 `MenuService` 클래스의 객체를 생성한 후, 이 객체로 `MenuService` 의 `getMenuList()` 메서드를 호출하고 있다.

이처럼 클래스끼리 사용하고자 하는 클래스의 객체를 생성해서 참조하게 되면 의존 관계가 성립된다.

### 의존성을 주입해보자

```java
public class CafeClient {
	public class void main(String[] args) {
		MenuSerivce menuService = new MenuService();
		MenuController controller = new MenuController(menuService);
		List<Menu> menuList = controller.getMenus();
```

```java
public class MenuController {
	private MenuService menuService;

	public MenuController(MenuService menuService) {
		this.menuService = menuService;
	}
	
	public List<Menu> getMenus() {
		return menuService.getMenuList();
	}
}
```

```java
public class MenuService {
	public List<Menu> getMenuList() {
		return null;
	}
}
```

이전 예시에서는 `MenuService` 의 기능을 사용하기 위해 `new` 키워드로 직접 생성한 반면에 위 코드에서는 `MenuControlloer` 생성자로 `MenuService` 의 객체를 전달 받고 있다.

이처럼 생성자를 통해 어떤 클래스의 객체를 전달 받는 것을 ‘의존성 주입’이라고 한다. **생성자의 파라미터로 객체를 전달하는 것을 외부에서 객체를 주입한다라고** 표현하는 것이다.

여기서 외부란 `CafeClient` 를 말한다. `MenuController` 의 생성자 파라미터로 `menuService` 를 전달하고 있기 때문이다.

정리해보자면 클래스의 생성자로 객체를 전달 받는 코드가 있다면 의존성 주입을 받고 있다고 볼 수 있다.

## Why(의존성 주입은 왜 필요할까?)

객체지향 언어인 Java에서 생성자를 통해 객체를 전달하는 일은 아주 흔한 일이다. 즉, 의존성 주입이 필요한 건 어찌보면 당연한 일인 것이다.

하지만 한 가지 염두에 두어야 하는 것이 있다. 바로 `new` 키워드를 쓸지 말지 여부를 결정하는 것이다. `new` 키워드를 사용하는 것이 당연하다고 생각할 수 있겠지만 **애플리케이션 코드 내부에서 직접적으로 `new` 키워드를 사용할 경우 객체지향 설계의 관점에서 중요한 문제가 발생할 수 있다.**

클라이언트의 요구 사항이 변경되었을 때, `new` 키워드를 사용해서 객체를 생성했을 경우, 이 클래스를 사용하는 모든 클래스들을 수정할 수 밖에 없다.

이처럼 `new` 키워드를 사용해 의존 객체를 생성할 대, 클래스들 간에 **강하게 결합(Tight Coupling)**되어 있다고 한다. 의존성 주입의 혜택을 보기위해서는 클래스들 간의 강한 결합은 피하고 느슨한 결합(Loose Coupling)이 필요하다.

애플리케이션의 요구사항은 언제든 변할 수 있기 때문에 코드를 작성할 때, **‘이렇게 작성해 놓으면 나중에 또 수정이 필요한 부분이 뭐가 있을까?’** 라고 생각해보는 것이 좋다. 

## How(느슨한 의존성 주입은 어떻게 할까?)\

Java에서 클래스들 간의 관계를 느슨하게 만드는 대표적인 방법은 **인터페이스(Interface)를 사용하는 것이다.**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6e20abfd-c74d-4a55-821f-0f4cad881a54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115322Z&X-Amz-Expires=86400&X-Amz-Signature=cb4b4541a197357db2ab32108ade3c66aa8738c197b65091dbd3789576e51759&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 그림을 보면 `MenuController` 가 `MenuService` 라는 클래스를 직접적으로 의존하는게 아니라 클래스 이름은 같지만 인터페이스를 의존하고 있다.

이처럼 어떤 클래스가 인터페이스 같이 일반화된 구성 요소에 의존하고 있을 때, 클래스들 간에 느슨하게 결합(Losse Coupling)되어 있다고 한다.

위 그림을 코드로 표현해보자

```java
public class CafeClient {
	public static void main(String[] args) {
		MenuService menuService = new MenuServiceStub();
		MenuController controller = new MenuController(menuService);
		List<Menu> menuList = controller.getMenu();
	}
}
```

```java
public class MenuController {
	private MenuService menuService;
	
	public MenuController(MenuService menuService) {
		this.menuService = menuService;
	}
	
	public List<Map> getMenus() {
		return menuService.getMenuList();
	}
}
```

```java
public interface MenuService {
	List<Menu> getMenuList();
}
```

```java
public class MenuServiceStub implements MenuService {
	@Override
	public List<Menu> getMenuList() {
		return List.of(
			new Menu(1, "아멜리카노", 2500),
			new Menu(2, "카라멜 마끼아또", 4500),
			new Menu(3, "바닐라 라떼", 4500)
		);
	}
}

```

- 위 코드를 보면 `MenuController` 가 생성자로 `MenuServiceStub` 클래스를 주입 받았지만 여기서는 주입 받은 대상이 `MenuService` 인터페이스이기 때문에 `MenuService` 인터페이스의 구현 클래스이면 어떤 클래스도 전부 주입을 받을 수 있다.
- (1)을 자세히 보면 `new` 로 `MenuServiceStub` 클래스의 객체를 생성해서 `MenuService` 인터페이스에 할당한다. 이처럼 인터페이스 타입의 변수에 그 인터페이스의 구현 객체를 할당할 수 있는데 이를 **업캐스팅(Upcasting)**이라고 한다.
- 업캐스팅을 통한 의존성 주입으로 인해 `MenuController` 와 `MenuService` 는 느슨한 결합 관계를 유지하게 되었다.
- 하지만 클래스들 간의 관계를 느슨하게 만들기 위해서는 `new` 키워드를 사용하지 않아야 하는데, CafeClient 클래스의 (1)을 보면 여전히 `new` 를 사용하고 있다. 이 `new` 키워드는 어떻게하면 제거하고 의존 관계를 느슨하게 만들 수 있을까? 그건 바로 Spring이 대신 해준다.

## Who(Spring에서 의존성 주입을 누가 해줄까?)

Spring 기반의 애플리케이션에서는 Spring이 의존 객체들을 주입해주기 때문에 애플리케이션 코드를 유연하게 구성할 수 있다.

## 핵심 포인트

- 애플리케이션 흐름의 주도권이 사용자에게 있지 않고, Framework이나 서블릿 컨테이너 등 외부에 있는 것 즉, 흐름의 주도권이 뒤바뀐 것을 IoC(Inversion of Control)라고 한다.
- DI(Dependency Injection)는 IoC개념을 조금 구체화시킨 것으로 객체 간의 관계를 느슨하게 해준다.
- 클래스 내부에서 다른 클래스의 객체를 생성하게 되면 두 클래스 간에 의존 관계가 성립하게 된다.
- 클래스 내부에서 `new` 를 사용해 참조할 클래스의 객체를 직접 생성하지 않고, **생성자 등을 통해 외부에서 다른 클래스의 객체를 전달 받고 있다면 의존성 주입이 이루어지고 있는 것**이다.
- `new` 키워드를 사용하여 객체를 생성할 때, 클래스 간에 강하게 결합(Tight Coupling)되어 있다고 한다.
- 어떤 클래스가 인터페이스같이 일반화된 구성 요소에 의존하고 있을 때, 클래스들 간에 느슨하게 결합(Loose Coupling)되어 있다고 한다.
- 객체들 간의 느슨한 결합은 요구 사항의 변경에 유연하게 대처할 수 있도록 해준다.
- 의존성 주입(DI)은 클래스들 간의 강한 결합을 느슨한 결합으로 만들어준다.
- Spring에서는 애플리케이션 코드에서 이루어지는 의존성 주입(DI)을 SPring에서 대신해준다.