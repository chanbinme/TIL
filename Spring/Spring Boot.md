# 목차
* [목차](#목차)
* [Spring Boot](#spring-boot)
    + [Spring Boot이란?](#spring-boot이란)
    + [Spring Boot을 사용해야 하는 이유](#spring-boot을-사용해야-하는-이유)
        + [XML 기반의 복잡한 설계 방식의 지양](#xml-기반의-복잡한-설계-방식-지양)
        + [의존 라이브러리의 자동 관리](#의존-라이브러리의-자동-관리)
        + [애플리케이션 설정의 자동 구성](#애플리케이션-설정의-자동-구성)
        + [프로덕션급 애플리케이션의 손쉬운 빌드](#프로덕션급-애플리케이션의-손쉬운-빌드)
        + [내장된 WAS를 통한 손쉬운 배포](#내장된-was를-통한-손쉬운-배포)
    + [Spring Boot의 핵심 컨셉](#spring-boot의-핵심-컨셉)
    + [핵심 포인트](#핵심-포인트)

# Spring Boot

## Spring Boot이란?

> Spring Boot은 Spring 설정의 복잡함으로 인해 Spring 기반 애플리케이션 개발을 시작하기도 전에 어려움을 겪는 문제점을 해결하기 위해 생겨난 Spirng Project 중 하나이다.
> 
- Spring Boot을 사용하면 기존에 겪었던 어려움을 겪지 않아도 되고, 손쉽게 그리고 빠르게 원하는 애플리케이션 제작을 할 수 있다.

## Spring Boot을 사용해야 하는 이유

- XML 기반의 복잡한 설계 방식의 지양
- 의존 라이브러리의 자동 관리
- 애플리케이션 설정의 자동 구성
- 프로덕션급 애플리케이션의 손쉬운 빌드
- 내장된 WAS를 통한 손쉬운 배포

### XML 기반의 복잡한 설계 방식 지양

Spring Boot 이전의 Spring 애플리케이션 개발을 위한 설정은 굉장히 복잡했다.

하지만 Spring Boot으로 인해 개발자는 Spring의 복잡한 설정에 대한 어려움으로부터 벗어날 수 있게 되었다.

### 의존 라이브러리의 자동 관리

Spring Boot이 나오기 전에는 애플리케이션에서 필요한 라이브러리를 사용하기 위해서는 필요한 라이브러리의 이름과 버전을 일일이 추가해 주어야 했다. 

이로 인해 라이브러리 간의 버전 불일치로 인한 빌드 및 실행 오류 역시 빈번하게 발생했다.

Spring Boot의 starter 모듈 구성 기능을 통해 의존 라이브러리를 수동으로 설정해야 하는 불편함이 사라졌다.

```java
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
  testImplementation 'org.springframework.boot:spring-boot-starter-test'
  implementation 'com.h2database:h2'
}
```

위 코드처럼 단 네줄만의 설정만으로 데이터베이스와의 연동은 물론 애플리케이션에 대한 모든 테스트가지 진행할 수 있다.

이처럼 Spring Boot을 사용하면 개발자가 의존 라이브러리를 직접 관리해야 하는 부담에서 벗어날 수 있다.

### 애플리케이션 설정의 자동 구성

Spring Boot은 Starter 모듈을 통해 설치되는 의존 라이브러리를 기반으로 애플리케이션의 설정을 자동으로 구성한다.

예를 들면,

- “`implementation 'org.springframework.boot:spring-boot-starter-web’`” 와 같은 starter가 존재한다면 애플리케이션이 웹 애플리케이션이라고 추측한 뒤, 웹 애플리케이션을 띄울 서블릿 컨테이너(디폴트: Tomcat) 설정을 자동으로 구성한다.
- “`implementation 'org.springframework.boot:spring-boot-starter-jdbc’`” 와 같은 starter가 존재한다면 애플리케이션에 데이터베이스 연결이 필요하다고 추측한 뒤, JDBC 설정을 자동으로 구성한다.

이처럼 Spring Boot에서 지원하는 자동 구성으로 인해 애플리케이션에 대한 설정을 직접해야하는 번거로움을 최소화 해준다.

이러한 자동 구성을 활성화 하기 위해서 우리가 해야할 일은 아래와 같은 애너테이션을 코드에 추가해주는 것 뿐이다.

```java
@SpringBootApplication // (1)
public class SampleApplication {
	public static void main(String[] args) {
		SpringApplication.run(SampleApplication.class, args);
	}
}
```

- (1)과 같이 `@SpringBootApplication` 애너테이션을 Spring 애플리케이션 코드에 추가해주면 Spring Boot에서 자동 구성 설정을 활성화해준다.

### 프로덕션급 애플리케이션의 손쉬운 빌드

Spring Boot을 사용하면 내가 개발한 애플리케이션 구현 코드를 손쉽게 빌드하여 내가 직접 빌드 결과물을 **War 파일 형태로 WAS(Web Application Server)에 올릴 필요가 없다.**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/645edbfc-28ca-4c58-ac1d-d72454246b63/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221012T054517Z&X-Amz-Expires=86400&X-Amz-Signature=17e44978f145b131e3774a42a343f5f186832d8bd930656b58e54df7cadd8ac3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Spring Boot에서 애플리케이션 빌드 명령을 실행하는 화면이다. bootJar 명령을 더블 클릭하면 아래와 같은 빌드 결과물이 생성된다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/59b5f7a2-c93d-466e-a9fe-493c3d5b7d48/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221012T054534Z&X-Amz-Expires=86400&X-Amz-Signature=287a23e05ea48fe76ff69cc7fd7ab075574494a2f9e7a5a633519acdac04567d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 그림과 같이 bootJar 명령을 실행해서 생성된 jar 파일은 즉시 시작 가능한 애플리케이션 실행 파일로 사용된다.

> 💡 **WAS(Web Application Server)란?**
>
> Java 기반의 웹 애플리케이션을 배포하는 일반적인 방식은 개발자가 구현한 애플리케이션 코드를 WAR(Web Application Archive)파일 형태로 빌드한 후에 WAS(Java에서는 서블릿 컨테이너라고도 부른다.)라는 서버에 배포해서 해당 애플리케이션을 실행하는 것이다.(Java 진영에서 사용되는 대표적인 WAS에는 Tomcat이 있다.)
>
> 즉, WAS는 구현된 코드를 빌드해서 나온 결과물을 실제 웹 애플리케이션으로 실행되게 해주는 서버이다.


### 내장된 WAS를 통한 손쉬운 배포

Spring Boot은 Apache Tomcat이라는 WAS를 내장하고 있기 때문에 별도의 WAS를 구축할 필요가 없으며, Spring Boot을 통해 빌드된 jar 파일을 이용해서 아래와 같은 명령어 한 줄만 입력해주면 서비스 가능한 웹 애플리케이션을 실행할 수 있다. 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/06169a0b-8fc1-403a-bbaf-b0e88ff71fba/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221012T054545Z&X-Amz-Expires=86400&X-Amz-Signature=d9e62e58e6021ad42a3759813ffdacda72c263769c009ab37a5428fd2f6f3607&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Spring Boot을 사용하면 ‘`java -jar <jar 파일명>.jar` 명령어를 통해 애플리케이션을 손쉽게 실행할 수 있다.

## Spring Boot의 핵심 컨셉

**“Spring 구성은 Spring에게 맡겨버리고 비즈니스 로직에만 집중하자!”**

## 핵심 포인트

- Spring Boot은 Spring 설정의 복잡함이라는 문제점을 해결하기 위해 생겨난 Spring Project 중 하나이다.
- Spring Boot을 사용해야 하는 이유
    - Spring Boot은 XML 기반의 복잡한 설계 방식을 지양한다.
    - Spring Boot의 starter 모듈 구성 기능을 통해 의존 라이브러리를 자동으로 구성해준다.
    - 애플리케이션 설정의 자동 구성
    - Spring Boot은 프로덕션급 애플리케이션의 빌드를 손쉽게 할 수 있다.
    - Spring Boot은 내장된 WAS를 사용 가능하기 때문에 배포가 용이하다.
- Spring Boot의 핵심 컨셉
    - Spring 구성은 Spring에게 맡기고 비즈니스 로직에만 집중하자!