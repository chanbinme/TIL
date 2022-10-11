* [목차](#목차)
* [AOP(Aspect Oriented Programming)](#aopaspect-oriented-programming)
    + [AOP(Aspect Oriented Programming)란?](#aopaspect-oriented-programming란)
        + [공통 관심 사항과 핵심 관심 사항](#공통-관심-사항과-핵심-관심-사항)
    + [AOP는 왜 필요할까?](#aop는-왜-필요할까)
        + [AOP가 적용되지 않은 코드](#aop가-적용되지-않은-코드)
        + [AOP가 적용된 코드](#aop가-적용된-코드)
    + [핵심 포인트](#핵심-포인트)

# AOP(Aspect Oriented Programming)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/478bf338-95b3-4d70-9661-6468a6d350c2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T120451Z&X-Amz-Expires=86400&X-Amz-Signature=5eccf5a738e7e8c433658f82778ffe422cb12de719bb61bbd8296ca1033b40ab&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

## AOP(Aspect Oriented Programming)란?

AOP는 관심지향 프로그래밍이다. 여기서 관심은 무엇을 말하는걸까?

사람마다 치킨을 먹는 사람들의 치킨 먹는 방식은 제각각 다를 수 있다. 어떤 사람은 손으로 집어 먹을 수도 있고 어떤 사람은 젓가락을 이용해 먹을 수도 있다. 그리고 어떤 사람은 치킨을 소금에 찍어 먹기도하고 어떤 사람은 양념에 찍어 먹기도 한다. 

이렇게 사람들마다 치킨 먹는 방식이 다를 수 있지만 공통되는 부분도 있다. 

그것 바로 맛있게 먹고 싶은 마음이다.

어떤식으로 치킨을 먹던지 먹는 방식과는 별개로 치킨을 맛있게 즐기고 싶은 것이 대부분 사람들의 공통된 관심사이다.

AOP에서의 Aspect는 이와 마찬가지로 애플리케이션에 필요한 기능 중에서 공통적으로 적용되는 공통 기능에 대한 관심과 관련이 있다. 

### 공통 관심 사항과 핵심 관심 사항

- 공통 관심 사항(Cross-cutting concern) : 애플리케이션 전반에 걸쳐 공통적으로 사용되는 기능(핵심 관심 사항에 반대되는 개념으로 공통 관심 사항을 부가적인 관심 사항이라고 표현하기도 한다.)
- 핵심 관심 사항(Core concern) : 비즈니스 로직. 즉, 애플리케이션의 주 목적을 달성하기 위한 핵심 로직에 대한 관심사

커피 전문점의 주인이 고객에게 제공하는 커피 메뉴를 구성하기 위해 커피 종류를 등록하는 것과 고객이 마시고 싶은 커피를 주문하는 기능은 애플리케이션의 **핵심 관심 사항**이다.

하지만 커피 주문 애플리케이션에 아무나 접속하지 못하도록 제한하는 애플리케이션 보안에 대한 부분은 애플리케이션 전반에 공통적으로 사용되는 기능이기때문에 **공통 관심 사항**이다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/192dc108-b621-49c2-b42d-0d859610decb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221011T120502Z&X-Amz-Expires=86400&X-Amz-Signature=cb1f50f10254fea6fc653dd0f3c892b78a2c6ca6f56ccb8494b20b9408bc047e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 그림을 보면 공통 관심 사항(로깅, 보안, 트랜잭션)이 애플리케이션의 핵심 관심 사항들을 관통하고 있다. 이것은 **공통 관심 사항의 기능들이 애플리케이션의 핵심 로직에 전반적으로 사용된다는 의미**이다.

그리고 그림에서 공통 관심 사항이 핵심 관심 사항에 머릴 떨어져 있는 것을 볼 수 있다. 이는 **공통 관심 사항이 핵심 관심 사항에서 분리되어 있다는 것을 의미**한다.

결국 **AOP라는 것은 애플리케이션의 핵심 업무 로직에서 로깅이나 보안, 트랜잭션 같은 공통 기능 로직들을 분리하는 것**이라고 볼 수 있다.

## AOP는 왜 필요할까?

애플리케이션의 핵심 로직에서 공통 기능을 분리하는 이유는 무엇일까?

- 코드의 간결성 유지
- 객체 지향 설계 원칙에 맞는 코드 구현
- 코드의 재사용

내 코드를 깔끔하고, 재사용이 가능하게 유지하려고 끊임 없이 노력하다보면 어느새 객체지향적인 사고에 익숙해져 있는 나를 볼 수 있을 것이다.

이번엔 AOP가 적용된 코드와 적용되지 않은 코드를 비교해보자

### AOP가 적용되지 않은 코드

```java
public class Example2_11 {
	private Connection connection;

	public void registerMemeber(Member member, Point point) throws SQLException {
		connection.setAutoCommit(false); // (1)
		try {
			saveMember(member); // (2)
			savePoint(point); // (2)

			connection.commit(); // (3)
		} catch (SQLException e) {
				connection.rollback(); // (4)
		}
	}
	private void saveMember(Member member) throws SQLException {
		PreparedStatement psMember = 
							connection.prepareStatement("INSERT INTO member (email, password) VALUES (?, ?)");
		psMember.setString(1, member.getEmail());
		psMember.setString(2, member.getPassword());
		psMember.executeUpdate();
	}

	private void savePoint(Point point) throws SQLException {
			PreparedStatement psPoint = 
								connection.prepareStatement("INSERT INTO point (email, point) VALUES (?, ?)");
			psPoint.setString(1, point.getEmail());
			psPoint.setInt(2, point.getPoint());
      psPoint.executeUpdate();
    }
}
```

위 코드는 작업을 트랜잭션으로 묶어서 처리하기 위한 기능들이다. 문제는 이렇게 트랜잭션 처리를 하는 코드들이 애플리케이션의 다른 기능에도 중복되어 나타난다는 것이다.

이럴 때, **중복된 코드를 공통화해서 재사용 가능하도록 만들어야 한다**. 그리고 이 공통화 작업은 **AOP**를 통해서 할 수 있다.

Spring에서는 이미 이런 트랜잭션 처리 기능을 AOP를 통해서 공통화 해두었다.

### AOP가 적용된 코드
```java
@Component
@Transactional // (1)
public class Example2_12 {
    private Connection connection;

    public void registerMember(Member member, Point point) throws SQLException {
        saveMember(member);
        savePoint(point);
    }

    private void saveMember(Member member) throws SQLException {
        // Spring JDBC를 이용한 회원 정보 저장
    }

    private void savePoint(Point point) throws SQLException {
        // Spring JDBC를 이용한 포인트 정보 저장
    }
}
```

Spring의 AOP 기능을 사용하여 `registerMember()` 에 트랜잭션을 적용한 예제 코드이다. 

Spring에서 트랜잭션 처리는 어떻게 하는 걸까? 바로 (1)의 `@Transactional` 애노테이션 하나만 붙이면 Spring 내부에서 이 애노테이션 정보를 활용해서 AOP 기능을 통해 트랜잭션을 적용한다.

**이처럼 AOP를 활용하면 애플리케이션 전반에 걸쳐 적용되는 공통 기능(트랜잭션, 로깅, 보안, 트레이싱, 모니터링) 등을 비즈니스 로직에서 깔끔하게 분리하여 재사용 가능한 모듈로 사용할 수 있다.**

## 핵심 포인트

- AOP(Aspect Oriented Programming)는 관심 지향 프로그래밍이다.
- AOP에서 의미하는 Aspect는 애플리케이션의 공통 관심사를 의미한다.
- 애플리케이션의 공통 관심사는 비즈니스 로직을 제외한 애플리케이션 전반에 걸쳐서 사용되는 공통 기능들을 의미한다.
- 애플리케이션 전반에 걸쳐서 사용되는 공통 기능에는 로깅, 보안, 트랜잭션, 모니터링, 트레이싱 등의 기능이 있다.
- AOP를 애플리케이션에 적용해서 다음과 같은 이점을 누릴 수 있다.
    - 코드의 간결성 유지
    - 객체 지향 설계 원칙에 맞는 코드 구현
    - 코드의 재사용
    