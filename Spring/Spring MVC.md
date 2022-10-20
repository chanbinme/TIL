* [목차](#목차)
* [Spring MVC](#spring-mvc)
    + [Spring MVC란?](#spring-mvc란)
        + [서블릿(Sevlet)이란?](#서블릿sevlet이란)
    + [Model](#model)
    + [View](#view)
        + [JSON(JavaScript Object Notation)이란?](#jsonjavascript-object-notation이란)
    + [Controller](#controller)
    + [Spring MVC 동작 순서](#spring-mvc-동작-순서)
    + [정리](#정리)

# Spring MVC

## Spring MVC란?

- 클라이언트의 요청을 편리하게 처리해주는 프레임워크

### 서블릿(Sevlet)이란?

- **클라이언트의 요청을 처리하도록 특정 규약에 맞추어서 Java 코드로 작성하는 클래스 파일**
- 아파치 톰캣(Apache Tomcat)은 이러한 서블릿들이 웹 애플리케이션으로 실행되도록 해주는 서블릿 컨테이너 중 하나

## Model

- Spring MVC 기반의 웹 애플리케이션이 클라이언트의 요청을 전달 받으면 요청 사항을 처리하기 위한 작업을 한다.
- 이렇게 처리한 작업의 결과 데이터를 클라이언트에게 응답으로 돌려줘야하는데, 이 때 클라이언트에게 응답으로 돌려주는 **작업의 처리 결과 데이터를 Model**이라고 한다.

>클라이언트의 요청 사항을 구체적으로 처리하는 영역을 서비스 계층(Service Layer)라고 하며, 실제로 요청 >사항을 처리하기 위해 Java 코드로 구현한 것을 비즈니스 로직(Business Logic)이라고 한다.


## View

- View는 Model 데이터를 이용해서 웹브라우저 같은 **클라이언트 애플리케이션의 화면에 보여지는 리소스(Resource)를 제공하는 역할**을 한다.
- Spring MVC에는 다양한 View 기술이 포함되어 있는데 View의 형태는 아래와 같이 나눌 수 있다.
    - HTML 페이지의 출력
        - 클라이언트 애플리케이션에 보여지는 HTML 페이지를 직접 렌더링해서 클라이언트 측에 전송하는 방식
        - 즉, 기본적인 HTML 태그로 구성된 페이지에 Model 데이터를 넣은 후, 최종적인 HTML 페이지를 만들어 클라이언트 측에 전송해준다.
        - Spring MVC에서 지원하는 HTML 페이지 출력 기술에는 Thymeleaf, FreeMarket, JSP, +JSTL, Tiles 등이 있다.
    - PDF, Excel 등의 문서 형태로 출력
        - Model 데이터를 가공해서 PDF 문서나 Excel 문서를 만들어서 클라이언트 측에 전송하는 방식
        - 문서 내에서 데이터가 동적으로 변경되어야 하는 경우 사용할 수 있는 방식이다.
    - XML, JSON 등 특정 형식의 포맷으로 변환
        - Model 데이터를 특정 프로토콜 형태로 변환해서 변환된 데이터를 클라이언트 측에 전송하는 방식
        - 이 방식의 경우 특정 형식의 데이터만 전송하고, 프론트엔드 측에서 이 데이터를 기반으로 HTML 페이지를 만든다.
        - 장점
            - 프론트엔드 영역과 백엔드 영역이 명확하게 구분되므로 개발 및 유지보수가 상대적으로 용이하다.
            - 프론트엔드 측에서 비동기 클라이언트 애플리케이션을 만드는 것이 가능해진다.
        
### JSON(JavaScript Object Notation)이란?

- JSON은 **Spring MVC에서 클라이언트 애플리케이션과 서버 애플리케이션이 주고 받는 데이터 형식**을 말한다.
- 과거에는 XML형식의 데이터가 많이 사용되었으나 현재는 XML보다 상대적으로 가볍고, 복잡하지 않은 JSON 형식을 대부분 사용하고 있는 추세이다.
- JSON의 기본 포맷
    - `{"속성":"값"}`
- 예제

```java
public class Coffee {
	private String korName;
	private String engName;
	private int price;

	public Coffee(String korName, String engName, int price) {
		this.korName = korName;
		this.engName = engName;
		this.price = price;
	}
}
```

```java
public class JsonExample {
	public static void main(String[] args) {
		Coffee coffee = new Coffee("아메리카노", "Americano", 3000);
		Gson gson = new Gson();
		String jsonString = gson.toJson(coffee);

		System.out.println(jsonString);
	}
}

// 결과
{"korName":"아메리카노","engName":"Americano","price":3000}
```

## Controller

- Controller는 클라이언트 측의 요청을 직접적으로 전달 받는 엔드포인트(Endpoint)로써 Model과 View의 중간에서 상호 작용을 해주는 역할이다.
- 즉, 클라이언트의 요청을 받아서 비즈니스 로직을 거친 후 Mdel 데이터가 만들어지면, 이 Model 데이터를 View로 전달하는 역할이다.

```java
@RestController
@RequestMapping(path = "/v1/coffee")
pbulic class CoffeeController {
	private final CoffeeService coffeeSerivce;

	CoffeeController(CoffeSerivce coffeeService) {
		this.coffeeService = coffeeService;
	}
	
	@GetMapping("/{coffee-id}")
	public Coffee getCoffee(@PathVariable("coffee-id") long coffeeId) {
		return coffeeService.findCoffee(coffeeId);
	}
}
```

- `@GetMapping` 애노테이션을 통해 클라이언트 측의 요청을 수신한다.
- `CoffeeService` 클래스의 `findCoffee()` 메서드를 호출해서 비즈니스 로직을 처리한다.
- `return coffeeService.findCoffee(coffeeId);`에서 비즈니스 로직을 처리한 다음 리턴 받는 Coffee가 Model 데이터가 된다.
- `getCoffee()` 에서 Model 데이터를 리턴하고 우리가 코드로 확인할 수는 없지만 Spring의 View가 전달 받아서 JSON 포맷으로 변경한 후 클라이언트 측에 전달한다.

## Spring MVC 동작 순서

Client가 요청 데이터 전송 → Controller가 요청 데이터 수신 → 비즈니스 로직 처리 → Model 데이터 생성 → Controller에게 Model 데이터 전달 → Controller가 View에게 Model 데이터 전달 → View가 응답 데이터 생성

## 정리

- Spring의 모듈 중에서 서블릿(Servlet) API를 기반으로 클라이언트의 요청을 처리하는 모듈이 바로 `spring-webmbc` 이다.
- spring-webmvc 모듈이 Spring MVC이다.
- Spring MVC는 웹 프레임워크의 한 종류이기 때문에 Spring MVC 프레임워크라고도 부른다.
- Spring MVC에서 M(Model)은 클라이언트에게 응답으로 돌려주기는 작업의 처리 결과 데이터를 Model이라고 한다.
- Spring MVC에서 V(View)는 Model 데이터를 이용해 웹브라우저 같은 애플리케이션의 화면에 보여지는 리소스(Resource)를 제공한다. 대표적인 포맷으로는 JSON이 있다.
- Spring MVC에서 C(Controller)는 클라이언트 측의 요청을 받아 Model과 View 중간에서 상호 작용을 해준다.
- Spring MVC의 전체적인 동작 흐름
    - Client가 요청 데이터 전송 → Controller가 요청 데이터 수신 → 비즈니스 로직 처리 → Model 데이터 생성 → Controller에게 Model 데이터 전달 → Controller가 View에게 Model 데이터 전달 → View가 응답 데이터 생성