* [목차](#목차)
* [IoC(Inversion of Control)](#iocinversion-of-control)
    + [IoC(Inversion of Control)란?](#iocinversion-of-control란)
        + [Java 콘솔 애플리케이션의 일반적인 제어권](#java-콘솔-애플리케이션의-일반적인-제어권)
        + [Java 웹 애플리케이션에서 IoC가 적용된 예](#java-웹-애플리케이션에서-ioc가-적용된-예)

# IoC(Inversion of Control)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1e5d0ec7-b273-4fce-b930-6d32f5116f54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115037Z&X-Amz-Expires=86400&X-Amz-Signature=ec75d37b0aa7101ad8673beb831d65358ef7f1959c2cee54eb420fd834e81129&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## IoC(Inversion of Control)란?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/32c56b15-6ea8-45ba-b870-a6c03241e5f2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115024Z&X-Amz-Expires=86400&X-Amz-Signature=ef1bca7e2bc9aff5d2acb8406394fb050db7fdc07f85a789eb5f12d1c873d729&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

IoC란 **애플리케이션 흐름의 주도권이 뒤바뀐 것**을 말한다.

IoC를 이해하기 위해서 기존에 우리가 알고 있던 애플리케이션의 흐름과 그 반대 흐름의 의미를 샘플 코드를 통해 알아보자

### Java 콘솔 애플리케이션의 일반적인 제어권

```java
public class Example2_10 {
	public static void main(String[] args) {
		System.out.println("Hello IoC!");
	}
}
```

일반적으로 위와 같은 Java 콘솔 애플리케이션을 실행하려면 `main()` 메서드가 있어야 한다. `main()` 메서드가 호출되고 난 다음에 `System` 클래스를 통해서 `static` 멤버 변수인 `out` 의 `println()` 을 호출한다.

이렇게 개발자가 작성한 코드를 순차적으로 실행하는게 애플리케이션의 일반적인 제어 흐름이다. 

### Java 웹 애플리케이션에서 IoC가 적용된 예

그렇다면 이번에는 Java 콘솔 애플리케이션이 아니라 웹 상에서 돌아가는 Java 웹 애플리케이션의 경우를 보자

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/568bf20f-5ee7-484f-ac0c-483e67c576d3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115050Z&X-Amz-Expires=86400&X-Amz-Signature=2e094d4c05c42fc09af8ac2503fca834c3f132fbc0f3328b18b48e5d972fc97c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 이미지는 서블릿 기반의 애플리케이션을 웹에서 실행하기 위한 서블릿 컨테이너의 모습이다. 

Java 콘솔 애플리케이션의 경우 `main()` 메서드가 종료되면 애플리케이션의 실행이 종료된다.

하지만 웹에서 동작하는 애플리케이션의 경우 클라이언트가 외부에서 접속해서 사용하는 서비스이기 때문에 `main()` 메서드가 종료되지 않아야 한다. 

그런데 서블릿 컨테이너는 서블릿 사양(Specification)에 맞게 작성된 서블릿 클래스만 존재하지 별도의 `main()` 메서드가 존재하지 않는다.

서블릿 컨테이너의 경우, 클라이언트의 요청이 들어올 때마다 서블릿 컨테이너 내의 컨테이너 로직(`service()` 메서드)이 서블릿을 직접 실행시켜주기 때문에 `main()` 메서드가 필요없다.

이 경우에는 서블릿 컨테이너가 서블릿을 제어하고 있기 때문에 애플리케이션의 주도권은 서블릿 컨테이너에 있다. 이 부분이 바로 서블릿과 웹 애플리케이션 간에 **IoC(제어의 역전)**의 개념이 적용되어 있는 것이다.

그렇다면 Spring에서는 이 IoC의 개념이 어떻게 적용되어 있을까?

바로 **DI(Dependency Injection)**이다.