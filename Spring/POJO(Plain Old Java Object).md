* [목차](#목차)
* [POJO(Plain Old Java Object)](#pojoplain-old-java-object)
    + [POJO(Plain Old Java Object)란?](#pojoplain-old-java-object란)
    + [POJO 프로그래밍이란?](#pojo-프로그래밍이란)
    + [POJO 프로그래밍이 필요한 이유](#pojo-프로그래밍이-필요한-이유)
    + [POJO와 Spring의 관계](#pojo와-spring의-관계)
    + [핵심 포인트](#핵심-포인트)
    

# POJO(Plain Old Java Object)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/33d9d983-ed13-4688-8de1-3e6d078baccd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T114649Z&X-Amz-Expires=86400&X-Amz-Signature=8a60b79b12207453cd6a66c4148c0e7209153e859a0f54489348673edcc4e823&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Spring 삼각형이라는 유명한 그림이다. 이 그림은 Spring의 핵심 개념들을 모두 표현하고 있다. 

그림에서 POJO는 Spring에서 사용하는 핵심 개념들에 둘러 싸여져있다. 이는 POJO라는 것을 IoC/DI, AOP, PSA를 통해서 달성할 수 있다는 것을 의미한다.

이 개념들을 하나씩 알아보자

## POJO(Plain Old Java Object)란?

POJO란 Java로 생성하는 순수한 객체를 의미한다. 그런데 ‘Java 문법으로 만든 객체는 당연히 Java 객체잖아?’ 라고 생각이 들텐데 프로그래밍 관점에서 조금 더 깊은 의미가 있다.

## POJO 프로그래밍이란?

POJO프로그래밍이란 POJO를 이용해서 프로그래밍 코드를 작성하는 것을 의미한다. 그런데 순수 자바 객체만을 사용해서 프로그래밍 코드를 작성한다고 해서 POJO 프로그래밍이라고 볼 수는 없다.

POJO 프로그래밍으로 작성한 코드라고 불리기 위해서는 크게 두 가지 규칙을 지켜주어야 한다.

- **Java나 Java의 스펙(사양)에 정의된 것 이외에는 다른 기술이나 규약에 얽매이지 않아야 한다.**
    
    ```java
    public class User {
    	private String userName;
    	private String id;
    	private String password;
    
    	public String getUserName() {
    		return userName;
    	}
    	
    	public void setUserName(String userName) {
    		this.userName = userName;
    	}
    
    	public String getId() {
    		return id;
    	}
    
    	public void setId(String id) {
    		this.id = id;
    	}
    
    	public String getPassword() {
    		return password;
    	}
    
    	public void setPassword(String password) {
    		this.password = password;
    	}
    }
    ```
    
    위 코드는 자바에서 제공하는 기능만 사용하여 getter, setter만 가지고 있는 코드이다. 해당 크랠스의 코드에는 Java언어 이외에 특정한 기술에 종속되어 있지 않은 순수한 객체이기 때문에 POJO라고 부를 수 있다.
    
    ```java
    public calss MessageForm extends ActionForm { // (1)
    	
    	String message;
    
    	public String getMessage() {
    		return message;
    	}
    
    	public void setMessage(String message) {
    		this.message = message;
    	}
    }
    
    public class MessageAction extends Action { // (2)
    	public ActionForward execute(ActionMapping mapping, ActionForm form,
    		HttpServletRequest request, HttpServletResponse response)
            throws Exception {
    		
    		MessageForm messageForm = (MessageForm) form;
    		messageForm .setMessage("Hello World");
    		
    		return mapping.findForward("success");
    	}
    	
    }
    ```
    
    위 코드는 Java 코드가 특정 기술에 종속적인 예를 보여주기 위한 코드이다. ActionForm 클래스는 과거에 Struts라는 웹 프레임워크에서 지원하는 클래스이다. 
    
    (1)에서는 Struts라는 기술을 사용하기 위해서 ActionForm을 상속하고 있다.
    
    (2)에서는 역시 Struts 기술의 Action 클래스를 상속 받고 있다.
    
    이렇게 특정 기술을 상속해서 코드를 작성하게 되면 나중에 애플리케이션의 요구사항이 변경되서 다른 기술로 변경하려면 S**truts의 클래스를 명시적으로 사용했던 부분을 전부 다 일일이 제거하거나 수정해야 한다.**
    
    그리고 Java는 다중 상속을 지원하지 않기 때문에 ‘extends’ 키워드를 사용해서 한 번 상속을 하게 되면 **상위 클래스를 상속받아서 하위 클래스를 확장하는 객체지향 설계 기법을 적용하기 어려워지게 된다.**
    
- **특정 환경에 종속적이지 않아야 한다.**
    
    서블릿 기반의 웹 애플리케이션을 실행시키는 서블릿 컨테이너인 아파치 톰캣을 예로 들어보자
    
    순수 Java로 작성한 애플리케이션 코드 내에서 Tomcat이 지원하는 API를 직접 가져다가 사용한다고 가정해보자. 
    
    그런데 만약 시스템의 요구 사항이 변경되어서 톰캣말고 제티라는 다른 서블릿 컨테이너를 사용하게 되면 어떻게 될까?
    
    **애플리케이션 코드에서 사용하고 있는 Tomcat API 코드들을 모두 걷어내고 Zetty로 수정하든가 최악의 경우에는 애플리케이션을 전부 뜯어 고쳐야될지도 모르는 상황에 직면하게 될 수 있다.**
    

## POJO 프로그래밍이 필요한 이유

- 특정 환경이나 기술에 종속적이지 않으면 재사용이 가능하고, 확장 가능한 유연한 코드를 작성할 수 있다.
- 저수준 레벨의 기술과 환경에 종속적인 코드를 애플리케이션 코드에서 제거함으로써 코드가 깔끔해진다.
- 코드가 깔끔해지기 때문에 디버깅하기도 상대적으로 쉽다.
- 특정 기술이나 환경에 종속적이지 않기 때문에 테스트 역시 단순해진다.
- **객체지향적인 설계를 제한없이 적용할 수 있다.(핵심)**

## POJO와 Spring의 관계

Spring은 POJO 프로그래밍을 지향하는 Framework이다.

그리고 최대한 다른 환경이나 기술에 종속적이지 않도록 하기 위한 POJO 프로그래밍 코드를 작성하기 위해서 Spring에서는 세가지 기술을 지원하고 있다. 

- IoC/DI
- AOP
- PSA

## 핵심 포인트

- POJO란 순수한 Java 객체를 의미한다.
- POJO 프로그래밍이란 순수 Java 객체가 다른 기술이나 환경에 종속되지 않도록 하기 위한 프로그래밍 기법이다.
- POJO 프로그래밍을 효과적으로 적용하기 위해서는 특정 기술에 대한 지식보다는 JDK의 API에 대한 지식과 객체지향적인 사고방식과 설계를 위한 훈련이 우선시 되어야 한다.
- Spring Famerwork은 POJO 프로그래밍을 지향하기 위해 IoC/DI, AOP, PSA라는 기술을 제공한다.