* [목차](#목차)
* [JDBC(Java Database Connectivity)](#jdbcjava-database-connectivity)
    + [JDBC란?](#jdbc란)
    + [JDBC API를 알아야 하는 이유](#jdbc-api를-알아야-하는-이유)
    + [JDBC의 동작 흐름](#jdbc의-동작-흐름)
    + [Connect Pool이란?](#connection-pool이란)
    + [핵심 포인트](#핵심-포인트)

# JDBC(Java Database Connectivity)

## JDBC란?

> JDBC는 Java 기반 애플리케이션의 코드 레벨에서 사용하는 데이터를 데이터베이스에 저장 및 업데이트하거나 반대로 데이터베이스에 저장된 데이터를 Java 코드 레벨에서 사용할 수 있도록 해주는 Java에서 제공하는 표준 사양(Specification)이다.
> 
- JDBC는 JDK1.1 버전부터 제공되던 표준 사양이다.
- JDBC API를 사용해서 다양한 벤더(Orcale, MS SQL, MySQL 등)의 데이터베이스와 연동할 수 있다.

## JDBC API를 알아야 하는 이유

- Spring에서는 JDBC API를 직접적으로 사용하기보다는 Spring Data JDBC나 Spring Data JPA 같은 기수을 제공함으로써 편리하게 데이터 액세스 로직을 구현할 수 있도록 해준다.
- 하지만, Spring Data JDBC나 Spirng Data JPA는 내부적으로 JDBC를 이용하기 때문에 JDBC의 동작 흐름 정도는 알면 Spring에서 지원하는 데이터 액세스 기술을 사요하는데 도움이 된다.

## JDBC의 동작 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f3ef0609-093c-41bd-b792-88842879d5d6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221027T131710Z&X-Amz-Expires=86400&X-Amz-Signature=c310671ef025cb78d3896f3cab43eb6bd12cbefe559657886d444a5ba0e0ce1f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JDBC는 Java 애플리케이션 내에서 JDBC API를 사용하여 데이터베이스 액세스하는 단순한 구조이다.
- JDBC API를 사용하기 위해서는 JDBC 드라이버를 먼저 로딩한 후에 데이터베이스와 연결하게 된다.

### JDBC 드라이버(JDBC Driver)

- JDBC 드라이버는 데이터베이스와의 통신을 담당하는 인터페이스이다.
- Oracle, MS SQL, MySQL 같은 다양한 벤더에서는 해당 벤더에 맞는 JDBC드라이버를 구현해서 제공을 하고, 우리는 이 JDBC 드라이버의 구현체를 이용해서 특정 벤더의 데이터베이스에 액세스 할 수 있다.

## JDBC API 사용 흐름

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8f7721be-a03f-47c5-b0b4-0263d3b169ab/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221027T131725Z&X-Amz-Expires=86400&X-Amz-Signature=4ed5291169a86a0ee5abd11689f943cb2fcb7deb22c1d04682ef09d886a99d12&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

1. JDBC 드라이버 로딩
    
    : 사용하고자 하는 JDBC 드라이버를 로딩한다. JDBC 드라이버는 DriverManager라는 클래스를 통해서 로딩된다.
    
2. Connection 객체 생성
    
    : JDBC 드라이버가 정상적으로 로딩되면 DriverManager를 통해 데이터베이스와 연결되는 세션(Session)인 Connection 객체를 생성한다.
    
3. Statement 객체 생성
    
    : Statement 객체는 작성된 SQL 쿼리문을 실행하기 위한 객체로써 객체 생성 후에 정적인 SQL 쿼리 문자열을 입력으로 가진다.
    
4. Query 실행
    
    : 생성된 Statement 객체를 이용해서 SQL 쿼리를 실행한다.
    
5. ResultSet 객체로부터 데이터 조회
    
    : 실행된 SQL 쿼리문에 대한 결과 데이터 셋이다.
    
6. ResultSet 객체 Close, Statement 객체 Close, Connection 객체 Close
    
    : JDBC API를 통해 사용된 객체들은 사용 이후에 사용한 순서의 역순으로 차례로 Close를 해줘야한다.
    

## Connection Pool이란?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/24072e33-39ee-48be-94e2-33c79e365c49/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221027T131744Z&X-Amz-Expires=86400&X-Amz-Signature=4cdef6d4b202abee1c664ed3f7afb0d23c5abd467d9b9ef75a297c4ae6d9e6df&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JDBC API를 사용해서 데이터베이스와의 연결을 위한 Connection 객체를 생성하는 작업은 비용이 많이 드는 작업 중 하나다.
- 이 부분을 해결하기 위해 애플리케이션 로딩 시점에 Connection 객체를 미리 생성해두고 애플리케이션에서 데이터베이스에 연결이 필요한 경우, Connection 객체를 새로 생성하는 것이 아니라 미리 만들어 둔 Connection 객체를 사용함으로써 애플리케이션의 성능을 향상 시킬 수 있다.
- 이처럼 데이터베이스 Connection을 미리 만들어서 보관하고 애플리케이션이 필요할 때 이 Connection을 제공해주는 역할을 하는 Connection 관리자를 Connection Pool이라고 한다.
- Spring Boot 2.0부터 HikariCP가 기본 DBCP로 채택되었다.

## 핵심 포인트

- JDBC(Java Database Connectivity)는 Java 기반 애플리케이션의 코드 레벨에서 사용하는 데이터를 데이터베이스에 저장 및 업데이트하거나 반대로 데이터베이스에 저장된 데이터를 Java 코드 레벨에서 사용할 수 있도록 해주는 Java에서 제공하는 표준 API이다.
- 데이터베이스 Connection 객체를 미리 만들어서 보관하고 애플리케이션이 필요할 때 이 Connection을 제공해주는 역할을 하는 Connection 관리자를 Connection Pool이라고 한다.
- Spring Boot 2.0부터 HikariCP가 기본 DBCP로 채택되었다.