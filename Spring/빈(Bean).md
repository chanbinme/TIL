* [목차](#목차)
* [빈(Bean)](#빈bean)
    + [Bean 접근 방법](#bean-접근-방법)
    + [BeanDefinition](#beandefinition)
        + [BeanDefintion 개체가 포함하고 있는 메타데이터](#beandefintion-개체가-포함하고-있는-메타데이터)
    + [핵심 포인트](#핵심-포인트)
* [빈 스코프(Bean Scope)](#빈-스코프bean-scope)
    + [싱글톤(singleton) 스코프](#싱글톤singleton-스코프)

# 빈(Bean)

> Spring 컨테이너가 관리하는 자바 객체를 의미하며, 하나 이상의 Bean을 관리한다.
> 
- 빈(Bean)은 인스턴스화된 객체를 의미한다.
- 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다.
- @Bean이 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다.
- 빈은 클래스의 등록정보, 게터/세터 메서드를 포함한다.
- 빈은 컨테이너에 사용되는 설정 메타데이터로 생성된다.
- 설정 메타데이터
    - XML 또는 자바 애너테이션, 자바 코드로 표현한다.
    - 컨테이너의 명령과 인스턴스화, 설정, 조립할 객체를 정의한다.

## Bean 접근 방법

- ApplicationContext를 사용해서 Bean의 정의를 읽고 액세스할 수 있다.

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

PetStoreService service = context.getBean("memberRepository", memberRepository.class);

List<String> userList = service.getUsernameList();
```

- `getBean()` 으로 Bean의 인스턴스를 가져올 수 있다.
- ApplicationContext 인터페이스에는 Bean을 가져오는 몇 가지 방법들이 있다.
- 실제적으로 응용 프로그램 코드에서는 getBean() 메서드로 호출해서 사용하면 안된다.

## BeanDefinition

- 스프링은 다양한 설정 형식을 `BeanDefinition` 이라는 추상화를 통해 지원할 수 있다.
- Bean은 BeanDefinition(빈 설정 메타정보)으로 정의되고 BeanDefinition에 따라서 활용하는 방법이 달라진다.
- 속성에 따라 컨테이너가 Bean을 어떻게 생성하고 관리할지 결정한다.
- @Bean 또는 Bean 당 각  1개씩 메타 정보가 생성된다.
- Spring이 설정 메타정보를 BeanDefinition 인터페이스를 통해 관리하기 때문에 컨테이너 설정을 XML, Java로 할 수 있다.
- 스프링 컨테이너는 설정 형식이 XML인지 Java 코드 형식인지 알 수 없고 BeanDefinition만 알면 된다.

### ****BeanDefintion 개체가 포함하고 있는 메타데이터****

- BeanClassName : 생성할 빈의 클래스 명(자바 설정처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName : 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
- factoryMethodName : 빈을 생성할 팩토리 메서드 지정, 예) userService
- Scope : 싱글톤(기본값)
- lazyInit : 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때까지 최대한 생성을 지연처리 하는지 여부
- InitMethodName : 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestoryMethodName : 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties : 의존관계 주입에서 사용한다. (자바 설정처럼 팩터리 역할의 빈을 사용하면 없음)

## 핵심 포인트

- Bean 하나에 하나씩 메타 정보가 생성되고, 스프링 컨테이너는 이런 메타 정보를 기반으로 스프링 Bean을 생성한다.

# 빈 스코프(Bean Scope)

> 빈(Bean)이 존재할 수 있는 범위를 의미한다.
> 
- 특정 Bean 정의에서 생성된 개체 연결할 다양한 의존성 및 구성 값뿐만 아니라 특정 Bean 정의에서 생성된 객체의 범위도 제어할 수 있다.
- Spring Framework는 6개의 범위를 지원하며, 그 중 4개는 ApplicationContext를 사용하는 경우에만 사용할 수 있다.
- Bean은 여러 범위 중 하나에 배치되도록 정의할 수 있다.
- 구성을 통해 생성하는 개체의 범위를 선택할 수 있기 때문에 강력하고 유연한다.
- 사용자 정의 범위를 생성할 수도 있다.

| Scope | Description |
| --- | --- |
| singleton | (Default) 각 Spring 컨테이너에 대한 단일 객체 인스턴스에 대한 단일 bean definition의 범위를 지정한다. |
| prototype | 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프이다. |
| request | 웹 요청이 들어오고 나갈때 까지 유지되는 스코프이다. |
| session | 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프이다. |
| application | 웹의 서블릿 컨텍스와 같은 범위로 유지되는 스코프이다. |
| websocket | 단일 bean definition 범위를 WebSocket의 라이프사이클까지 확장합니다. Spring ApplicationContext의 컨텍스트에서만 유효하다. |

싱글톤 스코프에 대해서만 알아보자

## 싱글톤(singleton) 스코프

> 싱글톤은 해당 Bean의 인스턴스를 오직 하나만 생성해서 사용하는 것을 말한다.
> 
- 스프링 컨테이너의 시작과 함께 생성되어서 스프링 컨테이너가 종료될 때까지 유지된다.
- 싱글톤은 하나의 Bean 공유 인스턴스만 관리하게 된다.
    - private 생성자를 사용해 외부에서 임의로 `new` 를 사용하지 못하도록 막아야 한다.
- 스프링 컨테이너 종료 시 소멸 메소드도 자동으로 실행된다.
- 단일 인스턴스는 싱글톤 Bean의 캐시에 저장된다.
- 해당 Bean Definition과 일치하는 ID 또는 ID를 가진 Bean에 대한 모든 요청은 스프링 컨테이너에서 해당 특정 Bean 인스턴스를 반환한다.
    - 싱글톤 스코프의 스프링 Bean은 여러번 호출해도 모두 같은 인스턴스 참조 주소값을 가진다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7b559289-f4b6-453b-b219-bd4c3000e301/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221013T135923Z&X-Amz-Expires=86400&X-Amz-Signature=ba009928f0fd740243e5bee5833b39945d039bed0e2b32624fc1d369ad467393&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

