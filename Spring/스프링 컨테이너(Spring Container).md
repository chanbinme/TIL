* [목차](#목차)
* [스프링 컨테이너(Spring Container)](#스프링-컨테이너spring-container)
    + [스프링 컨테이너를 왜 사용할까?](#스프링-컨테이너를-왜-사용할까)
    + [스프링 컨테이너는 어떻게 사용할까?](#스프링-컨테이너는-어떻게-사용할까)
    + [스프링 컨테이너의 종류](#스프링-컨테이너의-종류)
        + [BeanFactory](#beanfactory)
        + [ApplicationContext](#applicationcontext)
    + [컨테이너 인스턴스화](#컨테이너-인스턴스화)
    + [핵심 포인트](#핵심-포인트)

# 스프링 컨테이너(Spring Container)

> 스프링 컨테이너는 내부에 존재하는 애플리케이션 Bean의 인스턴스화, 구성, 전체 생명 주기 및 제거를 관리한다.
> 
- `ApplicationContext` 는 스프링 컨테이너라고 하는 인터페이스이다.(정확히는 `BeanFactory`, `ApplicationContext` 를 구분해서 말한다.)

```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory, MessageSource, ApplicationEventPublisher, ResourcePatternResolver {

}
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/88cab006-a8fa-4eb3-b408-58a17e4cf47b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221013T135456Z&X-Amz-Expires=86400&X-Amz-Signature=baa389f07586f2843abdfd13d6054c66ebee74b895d2c2f53c67b395ece86028&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 스프링 컨테이너는 XML, 애너테이션 기반의 자바 설정 클래스로 만들 수 있다.
- Bean의 인스턴스화, 구성, 전체 생명 주기 및 제거까지 처리한다.
    - 컨테이너는 개발자가 정의한 Bean을 객체로 만들어 관리하고 개발자가 필요로 할 때 제공한다.
- 스프링 컨테이너를 통해 원하는 만큼 많은 객체를 가질 수 있다.
- 의존성 주입을 통해 애플리케이션의 컴포넌트를 관리한다.
    - **스프링 컨테이너는 서로 다른 Bean을 연결해 애플리케이션의 Bean을 연결하는 역할**을 한다.
    - 개발자는 모듈 간에 의존 및 결합으로 인해 발생하는 문제로부터 자유로울 수 있다.
    - 메서드가 언제, 어디서 호출되어야 하는지, 메서드를 호출하기 위해 필요한 매개 변수를 준비해서 전달하지 않는다.

## 스프링 컨테이너를 왜 사용할까?

- 이전에는 객체를 사용하기 위해서 `new` 키워드를 사용해야 했다.
- 애플리케이션에서 이러한 객체가 수없이 많이 존재하고 서로 참조하게 되면서 객체 간의 의존성이 높아졌다.
- 높은 의존성은 낮은 결합도와 높은 캡슐화가 핵심인 객체 지향 프로그래밍을 지키기 힘들게 했다.
- 객체 간의 의존성을 낮추기 위해 Spring 컨테이너를 사용하게 되었다.
- 기존에는 새로운 정책이 생기면 모든 변경 사항들을 직접 수정해야 했다.
- 객체 간 의존이 낮을 때는 직접 수정할 수 있었지만, 서비스가 거대해지면서 의존도가 높아지고 그만큼 변경해야 할 코드가 늘어났다.
- 스프링 컨테이너를 사용하면 구현 클래스에 있는 의존을 제거하고 인터페이스에만 의존하도록 설계할 수 있다.

## 스프링 컨테이너는 어떻게 사용할까?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0879cba4-5cab-4d9c-9f70-5d5edb523b1d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221013T135508Z&X-Amz-Expires=86400&X-Amz-Signature=fbcbf98df0419e7a191355383dc0415af05cd0ac6ce00c7f24dd751f1aa555b7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 스프링 컨테이너는 Configuration Metadata를 사용한다.
- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링에 있는  @Bean을 등록한다.
- new AnnotationConfigApplicationContext(**`구성정보.class`**)로 스프링에 있는 @Bean의 메서드를 등록한다.
- 애너테이션 기반의 자바 설정 클래스로 Spring을 만든다.
    
    ```java
    // Spring Container 생성
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(DependencyConfig.class);
    ```
    

## 스프링 컨테이너의 종류

### BeanFactory

- 스프링 컨테이너의 최상위 인터페이다.
- BeanFactory는 Bean을 등록하고 생성하고 조회하고 돌려주는 등 Bean을 관리하는 역할을 한다.
- `getBean()` 메소드를 통해 Bean을 인스턴스화할 수 있다.
- @Bean이 붙은 메소드의 명을 스프링 Bean의 이름으로 사용해 등록한다.

 

### ApplicationContext

- BeanFactory의 기능을 상속 받아 제공한다.
- Bean을 관리하고 검색하는 기능을 BeanFactory가 제공하고 그 외에 부가기능을 제공한다.
- 부가 기능
    - `MessageSource` : 메세지 다국화를 위한 인터페이스
    - `EnvironmentCapable` : 개발, 운영 등 환경변수 등으로 나눠 처리하고, 애플리케이션 구동 시 필요한 정보들을 관리하기 위한 인터페이스
    - `ApplicationEventPublisher` : 이벤트 관련 기능을 제공하는 인터페이스
    - `ResourceLoader` : 파일, 클래스 패스, 외부 등 리소스를 편리하게 조회

## 컨테이너 인스턴스화

- ApplicationContext 생성자에 제공된 위치 경로 또는 경로는 컨테이너가 로컬 파일 시스템, Java ClASSPATH 등과 같은 **다양한 외부 리소스로부터 구성 메타데이터를 로드할 수 있도록 하는 리소스 문자열**이다.

## 핵심 포인트

- 컨테이너는 먼저 객체를 생성하고 객체를 서로 연결한다.
- 객체를 설정하는 단계를 지나 마지막으로 생명주기 전반을 관리한다.
- 컨테이너는 객체의 의존성을 확인해 생성한 뒤 적절한 객체에 의존성을 주입한다.
- 스프링은 스프링 컨테이너를 통해 객체를 관리한다.
- 스프링 컨테이너에서 관리되는 객체를 빈(Bean)이라고 한다.