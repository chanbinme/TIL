* [목차](#목차)
* [Spring Data JDBC 기반의 도메인 엔티티 및 테이블 설계](#spring-data-jdbc-기반의-도메인-엔티티-및-테이블-설계)
    + [DDD(Domain Driven Design)란?](#ddddomain-driven-design란)
    + [도메인(Domain)이란?](#도메인domain이란)
    * [애그리거트(Aggregate)란?](#애그리거트aggregate란)
    * [애그리거트 루트(Aggregate Root)란?](#애그리거트-루트aggregate-root란)
        * [애그리거트 루트(Aggregate Root) 선정 기준](#애그리거트-루트aggregate-root-선정-기준)
    * [샘플 애플리케이션 도메인 엔티티 및 테이블 설계](#샘플-애플리케이션-도메인-엔티티-및-테이블-설계)
        * [도메인에서 애그리거트 루트 찾기](#도메인에서-애그리거트-루트-찾기)
        * [애그리거트 간의 관계](#애그리거트-간의-관계)
        * [엔티티 클래스 간의 관계](#엔티티-클래스-간의-관계)
        * [애그리거트를 왜 구분할까?](#애그리거트를-왜-구분할까)
        * [데이터베이스 테이블 설계](#데이터베이스-테이블-설계)
    * [핵심 포인트](#핵심-포인트)

# Spring Data JDBC 기반의 도메인 엔티티 및 테이블 설계

- Spring Data JDBC 기반의 데이터 액세스 계층을 연동하기 위해 제일 먼저 해야 할 일은 **데이터베이스의 테이블**과 **도메인 엔티티 클래스의 설계**이다.

## DDD(Domain Driven Design)란?

> DDD는 도메인 주도 설계라는 뜻으로 의미 그대로 도메인 위주의 설계 기법을 의미한다.
> 
- 성능, 생산성, 안정성 면에서 뛰어난 애플리케이션을 설계하기 위해 고민한 결과물 중 하나가 바로 DDD라고 볼 수 있다.

## 도메인(Domain)이란?

> 도메인이란 우리가 실제로 현실 세계에서 접하는 업무의 영역이다.
> 
- 애플리케이션 개발에서 흔하게 사용하는 도메인이란 용어는 주로 비즈니스적인 어떤 업무 영역과 관련있다.
- 예를 들어 배달 앱을 만들어야 한다면 고객이 음식을 주문하는 과정, 주문받은 음식을 처리하는 과정, 조리된 음식을 배달하는 과정 등의 도메인 지식(Domain Knowledge)들을 서비스 계층에서 비즈니스 로직으로 구현해야 하는 것이라고 할 수 있다.

## 애그리거트(Aggregate)란?

> 애그리거트 비슷한 업무 도메인들의 묶음을 말한다.
> 
- 배달 주문 앱에서 필요한 업무 즉, 도메인은 무엇이 있을까?
    - 회원
    - 주문
    - 음식
    - 배달
    - 결제
- 그럼 이 도메인을 조금 더 세분화 해보자

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/93ef21fc-6236-4340-acd9-87703d5ad008/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221028T060059Z&X-Amz-Expires=86400&X-Amz-Signature=e4af07c9293e57fdaddf29222cfe5bca10575b3ade405a155edae90e4491121e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 회원, 주문, 음식, 결제는 상위 수준의 도메인이고, 상위 수준의 도메인 하위에 있는 각각의 정보들을 하위 수준의 도메인이라고 할 수 있다.
- 여기서 애그리거트는 총 네 개가 된다.
    - 회원 애그리거트
    - 주문 애그리거트
    - 음식 애그리거트
    - 결제 애그리거트
- **애그리거트는 한마디로 비슷한 범주의 연관된 업무들을 하나로 그룹화해 놓은 그룹**이라고 생각하면 좋다.

## 애그리거트 루트(Aggregate Root)란?

> 애그리거트 루트란 하나의 애그리거트를 대표하는 도메인을 말한다.
> 
- 애그리거트 안에는 1개 이상의 도메인들이 있는데, 각각의 애그리거트에는 해당 애그리거트를 대표하는 도메인이 존재한다.
- 예를 들어 각 아파트마다 동대표가 1명씩 있다. 동대표는 동을 대표해서 의견을 전달하거나, 주거 환경도 개선하는 등 동에 관련된 일은 동대표를 거쳐서 이루어지는 경우가 많다. 여기서 동대표가 애그리거트 루트인 셈이다. 이처럼 애그리거트 루트(Aggregate Rott)를 어떤 특정 집단이나 그룹의 대표라고 생각하면 좋다.

### 애그리거트 루트(Aggregate Root) 선정 기준

- 배달 앱 애그리거트를 보면 애그리거트 내의 도메인들 중에서 다른 모든 도메인들과 직간접적으로 연관이 있는 도메인들이 있다.
    - 회원 애그리거트의 경우, 회원 포인트가 얼마인지 알려면 해당 포인트를 가진 회원 정보를 알아야 한다. 즉,회원 정보 도메인이 애그리거트 루트가 된다.
- 데이터베이스의 테이블 간 관계로 보자면, 애그리거트 루트는 부모 테이블이 되고, 애그리거트 루트가 아닌 다른 도메인들은 자식 테이블이 되는 셈이다.
- 애그리거트 루트의 기본키 정보를 다른 도메인들이 외래키 형태로 가지고 있다고 볼 수 있다.
- 관계형 데이터베이스에서 A테이블의 기본키를 B테이블이 가지고 있다면 A는 부모 테이블이 되고, B는 자식 테이블이 된다. B인 자식 테이블이 가지고 있는 A 테이블의 기본키를 외래키라고 한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cf9c36ba-735c-4996-958c-5c2f644e3975/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221028T060111Z&X-Amz-Expires=86400&X-Amz-Signature=dc8a4b1696d946b5ee543d97d131081888bd45eb41289bf6fea93485769c746f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

## 샘플 애플리케이션 도메인 엔티티 및 테이블 설계

### 도메인에서 애그리거트 루트 찾기

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6cb1ba65-457d-4402-97fb-169466808529/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221028T060147Z&X-Amz-Expires=86400&X-Amz-Signature=74825bf0ff84ae9618dad5b49d72b25cedc0772331cf2a0a09ac9eb2ec5994ab&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

위 도메인 모델로 설계를 해보자

### 애그리거트 간의 관계

- 회원 정보(Member)와 주문 정보(Orders)의 관계(1 대 N)
    - 한 명의 회원은 여러 번의 주문을 할 수 있다.
- 주문 정보(Orders)와 커피 정보의 관계(N 대 N)
    - 하나의 주문은 여러 종류의 커피를 가질 수 있다.
    - 하나의 커피는 여러 건의 주문에 속할 수 있다.
- N 대 N의 관계는 일반적으로 1 대 N, N 대 1의 관계로 재설계되기 때문에 아래와 같이 변경될 수 있다.
    - 주문 정보(Orders)와 주문 커피 정보(Order_Coffee) : 1 대 N
    - 주문 커피 정보(Order_Coffee)와 커피 정보(Coffee) : N 대 1

### 엔티티 클래스 간의 관계

애그리거트를 기준으로 도메인 엔티티 클래스 간의 관계를 클래스 다이어그램을 통해 표현해보자

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/80275dde-355a-4d0a-bf65-06acf0084afd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221028T060158Z&X-Amz-Expires=86400&X-Amz-Signature=c278fbea44dc6bedca27aa71eed2a6895207485978c1cd42f048d3679f68ef07&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 회원(Member) 엔티티 클래스
    - Member 클래스와 Order의 관계는 1 대 N의 관계이기 때문에 Member 클래스 `List<Order>` 추가

> 데이터베이스 테이블간의 관계는 외래키를 통해 맺어지지만 클래스 간의 관계는 객체의 참조를 통해 맺어진다.
> 
- 주문(Order) 엔티티 클래스
    - Order 클래스와 Coffee 클래스는 N 대 N의 관계를 가지기 때문에 N 대 N의 관계를 1 대 N의 관계로 만들어주는 `List<OrderCoffee>` 를 멤버 변수로 추가
- 커피(Coffee) 엔티티 클래스
    - Coffee 클래스와 Order 클래스는 N 대 N의 관계를 가지기 때문에 N 대 N의 관계를 1 대 N의 관계로 만들어주는 `List<OrderCoffee>` 를 멤버 변수로 추가
- 주문_커피(OrderCoffee) 테이블
    - Order 클래스와 Coffee 클래스가 N 대 N 관계이므로 두 클래스의 관계를 각각 1 대 N의 관계로 만들어주기 위한 OrderCoffee 클래스 추가
    - 주문하는 커피가 한 잔 이상일 수 있기 때문에 quantity(주문 수량) 멤버 변수를 추가

### 애그리거트를 왜 구분할까?

- ORM은 말 그대로 객체와 테이블을 매핑하는 기술이기 때문에 클래스 간의 연관 관계를 찾을 수 밖에 없다.
- Spring Data JDBC는 DDD와 밀접한 관련이 있기 때문에 Spring Data JDBC 기술을 사용하기 위해서는 도메인 모델을 잘 정의하고 애그리거트 루트(Aggregate Root)를 찾아야한다.
- 애그리거트 루트를 통해 프로그래밍 코드 사엥서의 구현 방법이 달라진다.

### 데이터베이스 테이블 설계

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/59ea4f0a-2cf4-4f7e-9eef-bf418a1713d6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221028T060212Z&X-Amz-Expires=86400&X-Amz-Signature=8bca9a6c76efad8f8fee47eaba3278feaddd464711ce4844871b2d91142fcce7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 도메인 엔티티 클래스 간의 관계는 객체의 참조로 이루어지지만 테이블 간의 관계는 외래키(Foreign key) 참조로 이루어진다.
- 주문 테이블의 이름이 ‘ORDER’이 아니라 ‘ORDERS’인 이유는 SQL 쿼리문에서 테이블의 로우(Row)를 정렬하기 위해 사용하는 ‘ORDER BY’라는 예약어에 사용되기 때문에 테이블 생성 시 에러를 발생할 수 있다.

## 핵심 포인트

- DDD(Domain Driven Design, 도메인 주도 설계)는 도메인 위주의 설계 기법이다.
- 도메인(Domain)이란?
    - 애플리케이션 개발에서 사용하는 도메인이란 용어는 주로 비즈니스적인 어떤 업무 영역과 관련이 있다.
    - 애플리케이션을 구현하기 위해 필요한 업무들을 자세히 알면 알수록 퀄리티가 높은 애플리케이션을 만들 가능성이 높다.
    - 비즈니스 업무 영역을 의미하는 도메인 지식(Domain Knwledge)들을 서비스 계층에서 비즈니스 로직으로 구현해야 한다.
- 애그리거트(Aggregate)와 애그리거트 루트(Aggregate Root)는 DDD에서 사용되는 용어이다.
    - 애그리거트란 비슷한 업무의 하위 수준 도메인들의 묶음을 의미한다.
    - 애그리거트 내의 대표 도메인을 DDD에서는 애그리거트 루트(Aggregate Root)라고 한다.
    - 애그리거트 루트의 선정 기준
        - 각 애그리거트 내의 도메인들 중에서 다른 모든 도메인들과 직간접적으로 연관이 되어 있는 도메인이 애그리거트 루트가 된다.
- 데이터베이스 테이블 간의 관계는 외래키를 통해 맺어지지만 클래스끼리 관계는 객체의 참조를 통해 관계가 맺어진다.