* [목차](#목차)
* [Framework와 Library의 차이](#framework와-library의-차이)
    + [자동차에서의 Frame과 Library](#자동차에서의-frame과-library)
    + [예제](#예제)

# Framework와 Library의 차이

많은 사람들이 Framework와 Library가 같은 의미라고 오해한다.(물론 나도 그랬다)

어찌보면 당연히 헷갈릴 수 밖에 없다. 이번엔 Framework와 Library의 차이에 대해 알아보자

- Framework : 기본적인 프로그래밍을 하기 위한 어떠한 틀이나 구조를 제공하는 것
- Library : 프로그래밍할 때 필요한 기능을 미리 구현해놓은 집합체

위 내용만으로 정확한 차이점을 파악하기 어렵다.

## 자동차에서의 Frame과 Library

애플리케이션을 자동차에 비유해보자. 자동차를 만들기 위해 필요한 것들은 어떤게 있을까?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/49f0c244-c25e-43a0-b75a-d54bbbb0137d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T115827Z&X-Amz-Expires=86400&X-Amz-Signature=b4cc8b9bea509c469f1708199a4ca387d68fff8708b37205521831ec60abb30a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Frame :  차체를 구성하는 자동차의 뼈대
- Library : 자동차에 다양한 기능을 제공하는 부품

자동차의 Frame은 쉽게 교체할 수 없다. 하지만 바퀴나 와이퍼, 라이트는 언제든지 쉽게 교체가 가능하다. 이러한 특징을 **소프트웨어 관점에서 보면 한 번 정해진 Framework를 교체하는 일은 어렵지만. Library는 쉽게 교체가 가능하며 필요한 Library들을 선택적으로 사용할 수 있다는 의미이다.**

정리해보자면 **애플리케이션에 대한 제어권의 차이**가 있다.

## 예제

```java
@SpringBootApplication
@RestController
@RequestMapping(path = "/v1/message")
pbulic class SampleApllication {
	@GetMapping
	public String getMassage() {  // (2)
		String message = "hello world";
		return StringUtils.upperCase(message); // (1)
	}

	public static void main(String[] args) {
		SpringApplication.run(SampleApplication.class, args);
	}
}
```

(1)은 파라미터로 전달하는 문자열을 대문자로 변환해주는 라이브러리다. 어떻게 (1)이 라이브러리라는 것을 알 수 있을까? 

애플리케이션 코드는 개발자가 작성한다. 이렇게 개발자가 짜 놓은 코드내에서 필요한 기능이 있으면 해당 라이브러리를 호출해서 사용하는 것이 바로 Library이다. 

즉, **애플리케이션 흐름의 주도권이 개발자에게 있는 것**이다.  

(2)의 `getMessage()` 는 Spring Framework에서 지원하는 기능인데 라이브러리와는 다르게 코드 상에는 보이지 않는 상당히 많은 일들을 한다. 

`getMessage()` 메서드 내부의 코드처럼 개발자가 메서드내에 코드를 작성해두면, Spring Framework에서 개발자가 작성한 코드를 사용해서 애플리케이션의 흐름을 만들어낸다. 

즉, **애플리케이션 흐름의 주도권이 개발자가 아닌 Framework에 있는 것**이다.

이 것이 Spring Framework의 핵심 개념인 IoC(Inversion Of Control, 제어의 역전다.

정리해보자면

- Framework는 개발자가 애플리케이션의 핵짐 로직을 개발하는 것에 집중할 수 있도록 해준다.
- Library는 애플리케이션 흐름의 주도권이 개발자에게게 있는 반면, Framework는 애플리케이션의 흐름의 주도권이 개발자가 아닌 Framework에 있다.