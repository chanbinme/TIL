* [목차](#목차)
* [JPA(Java Persistence API)](#jpajava-persistence-api)
    + [JPA란?](#jpa란)
        + [Hibernate ORM](#hibernate-orm)
    + [데이터 액세스 계층에서의 JPA 위치](#데이터-액세스-계층에서의-jpa-위치)
    + [JPA에서 P(Persistence)의 의미](#jpa에서-ppersistence의-의미)
        +[영속성 컨텍스트(Persistence Context)](#영속성-컨텍스트persistence-context)
    + [JPA API로 영속성 컨텍스트 이해하기](#jpa-api로-영속성-컨텍스트-이해하기)

# JPA(Java Persistence API)

## JPA란?

> Java에서 사용하는 ORM(Object-Relational Mapping) 기술의 표준 사양(또는 명세, Specifincation)
> 
- 표준 사양이라는 의미는 Java의 인터페이스로 사양이 정의되어 있기 때문에 JPA라는 표준 사양을 구현한 구현체는 따로 있다는 것을 의미한다.
- JPA 표준 사양을 구현한 구현체로는 Hibernate ORM, EclipseLink, DataNucleus 등이 있다.
- 이번에는 Hibernate ORM을 사용해보자

### Hibernate ORM

> Hibernate ORM은 JPA에서 정의해둔 인터페이스를 구현한 구현체로써 JPA에서 지원하는 기능 이외에 Hibernate 자체적으로 사용할 수 있는 API 역시 지원하고 있다.
> 

## 데이터 액세스 계층에서의 JPA 위치

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/02d4100d-5043-42cb-828c-a85540dbdd9b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061541Z&X-Amz-Expires=86400&X-Amz-Signature=5e1dcf8e33780df172278ec5d7848741810087dca24a363e89a2ee0919832d59&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- JPA는 데이터 액세스 계층의 상단에 위치한다.
- 데이터 저장, 조회 등의 작업은 JPA를 거쳐 Hibernate ORM을 통해 이루어지며 Hibernate ORM은 내부적으로 JDBC API를 이용해 데이터베이스에 접근한다.
- 따라서 데이터 액세스 계층 상단에 위치한 JPA에서 지원하는 API를 사용해서 데이터베이스에 접근할 수 있다.

## JPA에서 P(Persistence)의 의미

> Persistence는 영속성, 지속성이라는 뜻을 가지고 있다. 즉, 무언가를 금방 사라지지 않고 오래 지속되게 한다라는 목적을 가지고 있는 것이다.
> 

### 영속성 컨텍스트(Persistence Context)

- ORM은 **객체(Object)와 데이터베이스 테이블의 매핑을 통해 엔티티 클래스 객체 안에 포함된 정보를 테이블에 저장하는 기술**이다.
- JPA에서는 **테이블과 매핑되는 엔티티 객체 정보를 영속성 컨텍스트(Persistence Context)라는 곳에 보관해서 애플리케이션 내에 오래 지속** 되도록 한다.
- 이렇게 보관된 엔티티 정보는 데이터베이스 테이블에 데이터를 저장, 수정, 조회, 삭제하는데 사용된다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4372c225-474e-4d37-925a-b0fb89373e35/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061556Z&X-Amz-Expires=86400&X-Amz-Signature=d332c364d788c29066e82e85a04e6222ec222456a06e0fc2e9dbaa144120ad0f&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 그림은 영속성 컨텍스트를 그림으로 표현한 것이다. 영속성 컨텍스트에는 **1차 캐시**와 **쓰기 지연 SQL 저장소**라는 영역이 있다.
- JPA API 중에서 엔티티 정보를 영속성 컨텍스트에 저장(persist)하는 API를 사용하면 영속성 컨텍스트의 1차 캐시에 엔티티 정보가 저장된다.

## JPA API로 영속성 컨텍스트 이해하기

- JPA에서 지원하는 API를 사용해서 영속성 컨텍스트에 엔티티를 저장해보자

### ✔️ **build.gradle 설정**

```java
dependencies {
		...
		implementation 'org.springframework.boot:spring-boot-starter-data-jpa'		
}
```

- 위 내용을 추가하면 기본적으로 Spring Data JPA 기술을 포함해서 JPA API를 사용할 수 있다.

### **✔️ JPA 설정(application.yml)**

```sql
spring:
	h2:
		console:
			enabled: true
			path: /h2    # Context path
		datasource:
			url: jdbc:h2:mem:test    # JDBC URL
		jpa:
			hibernate:
				ddl-auto: create   # 스키마 자동 생성
			show-sql: true    # SQL 쿼리 출력
```

- `ddl-auto: create` : JPA에서 사용하는 엔티티 클래스를 정의하고 애플리케이션 실행 시, 이 엔티티와 매핑되는 테이블을 데이터베이스에 자동으로 생성해준다. 즉, **JPA가 자동으로 데이터베이스에 테이블을 생성해준다.**
- `show-sql: true` :  JPA의 동작 과정을 이해하기 위해 JPA API를 통해서 실행되는 SQL 쿼리를 로그로 출력해준다.

### **✔️ 영속성 컨텍스트에 엔티티 저장**

```java
package com.codestates.member.entity;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

/**
 * @Entity 애너테이션과 @Id 애너테이션을 추가해주면 JPA에서 해당 클래스를 엔티티 클래스로 인식한다.
 */
@Getter
@Setter
@NoArgsConstructor
@Entity
public class Member {
    /**
     * @GenerateValue 애너테이션은 식별자를 생성해주는 전략을 지정할 때 사용한다.
     * 식별자에 해당하는 멤버 변수에 @GeneratedValue 애너테이션을 추가하면 데이터베이스 테이블에서 기본키가 되는 식별자를 자동으로 설정해준다.
     **/
    @Id

    @GeneratedValue
    private Long memberId;
    private String email;

    public Member(String email) {
        this.email = email;
    }
}
```

**✔️샘플 코드 실행을 위한 Configuration 클래스 생성**

```java
package com.codestates;

import com.codestates.member.entity.Member;
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;

/**
 * @Configuration 애너테이션을 추가하면 Spring에서 Bean 검색 대상인 Configuration 클래스로 간주해서
 * @Bean 애너테이션이 추가된 메서드를 검색한 후, 해당 메서드에서 리턴하는 객체를 Spring Bean으로 추가해준다.
 */
@Configuration
public class JpaBasicConfig {
    /**
     * JPA의 영속성 컨텍스트를 관리하는 클래스이다.
     * EntityManager 클래스의 객체는 EntityMangerFactory 객체를 Spring으로부터 DI 받을 수 있다.
      */
    private EntityManager em;

    //
    private EntityTransaction tx;

    /**
     * CommandLineRunner 객체를 람다 표현식으로 정의해주면 애플리케이션 부트스트랩 과정이 완료된 후에
     * 람다 표현식에 정의한 코드를 실행해준다.
     */
    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();
        this.tx = em.getTransaction();

        /**
         * EntityManageFactory의 createEntityManager()메서드를 이용해서 EntityManager 클래스의 객체를 얻을 수 있다.
         * 이제 이 EntityManager 클래스의 객체를 통해서 JPA의 API메서드를 사용할 수 있다.
         */
        return args -> {
            example01();
        };
    }

    private void example01() {
        Member member = new Member("gksmfcksqls@gmail.com");

        // persist(member) 메서드를 호출하면 영속성 컨텍스트에 member 객체의 정보들이 저장된다.
        em.persist(member);

        /**
         *  find(Member.class, 1L) 메서드로 영속성 컨텍스트에 member 객체가 잘 저장되었는지 조회할 수 있다.
         *  첫 번째 파라미터는 조회할 엔티티 클래스의 타입이다.
         *  두 번째 파라미터는 조회할 엔티티 클래스의 식별자 값이다.
         */
        Member resultMember = em.find(Member.class, 1L);
        System.out.println("Id: " + resultMember.getMemberId() +
                ", email: " + resultMember.getEmail());
    }
}
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cb2d6bd5-68d7-4ff1-81bc-11f75195d93b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061616Z&X-Amz-Expires=86400&X-Amz-Signature=cc89c881d4c53527c5084e4ce0503df69d8358f0cfed200d2ad031195a1c2d42&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 그림은 지금까지의 코드를 실행했을 때의 영속성 컨텍스트의 상태이다.
- `em.persist(member)` 를 호출 → 1차 캐시에 `member` 객체가 저장 → `member` 객체는 쓰기 지연 SQL 저장소에 INSERT 쿼리 형태로 등록

```
Hibernate: drop table if exists member CASCADE 
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, email varchar(255), primary key (member_id))

Hibernate: call next value for hibernate_sequence
Id: 1, email: gksmfcksqls@gmail.com
```

- ID가 1인 Member의 email 주소를 영속성 컨텍스트에서 조회하고 있는 것을 확인할 수 있다.
- `member` 객체 정보를 출력하는 라인 윗쪽 로그에서 JPA가 내부적으로 테이블을 자동 생성하고, 테이블의 기본키를 할당해주는 것을 확인할 수 있다.
- `em.persist(member)` 를 호출할 경우, 영속성 컨텍스트에 `member` 객체를 저장하지만 실제 테이블에 회원 정보를 저장하지는 않는다.

### **✔️ 영속성 컨텍스트와 테이블에 엔티티 저장**

```java
@Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();

        /**
         * EntityManager를 통해서 Transaction 객체를 얻는다.
         * JPA에서는 이 Transaction 객체를 기준으로 데이터베이스의 테이블 데이터를 저장한다.
         */
        this.tx = em.getTransaction();

        /**
         * EntityManageFactory의 createEntityManager()메서드를 이용해서 EntityManager 클래스의 객체를 얻을 수 있다.
         * 이제 이 EntityManager 클래스의 객체를 통해서 JPA의 API메서드를 사용할 수 있다.
         */
        return args -> {
            example01();
        };
    }

		...

    private void example02() {

        // Transaction을 시작하기 위해서는 tx.begin() 메서드를 먼저 호출해주어야 한다.
        tx.begin();
        Member member = new Member("gksmfcksqls@gmail.com");

        // member 객체를 영속성 컨텍스트에 저장한다.
        em.persist(member);

        // tx.commit()을 호출하는 시점에 영속성 컨텍스트에 저장되어 있는 Member 객체를 데이터베이스의 테이블에 저장한다.
        tx.commit();

        // em.find(Member.class, 1L)을 호출하면 em.persist(member)에서 영속성 컨텍스트에 저장한 MEMBER 객체를 1차 캐시에서 조회한다.
        Member resultMember1 = em.find(Member.class, 1L);
        System.out.println("Id: " + resultMember1.getMemberId() +
                "email: " + resultMember1.getEmail());

        /**
         * em.find(Member.class, 2L)를 호출하지만 식별자가 2L인 member 객체는 존재하지 않기 때문에 결과는 true다.
         * 식별자가 2L인 member 객체가 존재하지 않기 때문에 테이블에 직접 SELECT 쿼리를 전송한다.(실행 결과 로그에서 확인 가능)
          */ 
        Member resultMember2 = em.find(Member.class, 2L);
        System.out.println(resultMember2 == null);
    }
}
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2a0f3661-91d0-44b0-abde-d756a189ca8f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061636Z&X-Amz-Expires=86400&X-Amz-Signature=28a13f65d13a185155ce482cd90cc7e81dfd9de2a46c58e15af18c577e171615&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 코드를 실행했을 때의 영속성 컨텍스트의 상태이다.
- `tx.commit()` 을 했기 때문에 `member` 에 대한 INSERT 쿼리는 실행되어 쓰기 지연 SQL 저장소에서 사라진다.

```
Hibernate: drop table if exists member CASCADE 
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, email varchar(255), primary key (member_id))

Hibernate: call next value for hibernate_sequence
Hibernate: insert into member (email, member_id) values (?, ?)
Id: 1, email: gksmfcksqls@gmail.com

Hibernate: select member0_.member_id as member_i1_0_0_, member0_.email as email2_0_0_ from member member0_ where member0_.member_id=?
true
```

- 맨 아래 로그를 보면 `em.find(Member.class, 2L)` 로 조회를 했는데 식별자 값이 2L에 해당하는 `member2` 객체가 영속성 컨텍스트의 1차 캐시에 없기 때문에 추가적으로

### 핵심 포인트

위 두 개의 코드에서 기억해야 할 내용은 다음과 같다.

- `em.persist()` 를 호출하면 영속성 컨텍스트의 1차 캐시에 엔티티 클래스의 객체가 저장되고, 쓰기 지연 SQL 저장소에 INSERT 쿼리가 등록된다.
- `tx.commit()` 을 하는 순간 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리가 실행되고, 실행된 INSERT 쿼리는 쓰기 지연 SQL 저장소에서 제거된다.
- `em.find()` 를 호출하면 먼저 1차 캐시에서 해당 객체가 있는지 조회하고, 없으면 테이블에 SELECT 쿼리를 전송해서 조회한다.

### **✔️ 쓰기 지연을 통한 영속성 컨텍스트와 테이블에 엔티티 일괄 저장**

```java
package com.codestates;

import com.codestates.member.entity.Member;
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;

/**
 * @Configuration 애너테이션을 추가하면 Spring에서 Bean 검색 대상인 Configuration 클래스로 간주해서
 * @Bean 애너테이션이 추가된 메서드를 검색한 후, 해당 메서드에서 리턴하는 객체를 Spring Bean으로 추가해준다.
 */
@Configuration
public class JpaBasicConfig {
    /**
     * JPA의 영속성 컨텍스트를 관리하는 클래스이다.
     * EntityManager 클래스의 객체는 EntityMangerFactory 객체를 Spring으로부터 DI 받을 수 있다.
      */
    private EntityManager em;

    //
    private EntityTransaction tx;

    /**
     * CommandLineRunner 객체를 람다 표현식으로 정의해주면 애플리케이션 부트스트랩 과정이 완료된 후에
     * 람다 표현식에 정의한 코드를 실행해준다.
     */
    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();

        /**
         * EntityManager를 통해서 Transaction 객체를 얻는다.
         * JPA에서는 이 Transaction 객체를 기준으로 데이터베이스의 테이블 데이터를 저장한다.
         */
        this.tx = em.getTransaction();

        /**
         * EntityManageFactory의 createEntityManager()메서드를 이용해서 EntityManager 클래스의 객체를 얻을 수 있다.
         * 이제 이 EntityManager 클래스의 객체를 통해서 JPA의 API메서드를 사용할 수 있다.
         */
        return args -> {
            example03();
        };
    
 
    private void example03() {
        tx.begin();

        Member member1 = new Member("gksmfcksqls@gmail.com");
        Member member2 = new Member("Ekdcksqls@gmail.com");

        // 각각 member1과 member2 객체를 영속성 컨텍스트에 저장
        em.persist(member1);
        em.persist(member2);

        tx.commit();
    }
}
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/245d1275-9c22-40bc-8219-92f0a77e0086/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061652Z&X-Amz-Expires=86400&X-Amz-Signature=a1076360356f628c1e5f80941b1ca4b84448e03b2458e87c3f8bdceef869167d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 그림은 위 코드에서 `tx.commit()` 이 실행되기 직전의 영속성 컨텍스트 상태를 표현한 것이다.
- `tx.commit()` 을 하기 전까지는 `em.persist()` 를 통해 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리가 실행이 되지 않는다. 따라서 테이블에 데이터가 저장되지 않는다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6b01ee67-d109-4af5-bcf0-22f797dd234a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T061715Z&X-Amz-Expires=86400&X-Amz-Signature=2598397447bb2b31f534144a8db611f443b745436e73e67538644cb24a69fc18&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 위 그림은 위 코드에서 `tx.commit()` 이 실행된 직후의 영속성 컨텍스트 상태를 표현한 것이다.
- `tx.commit()` 이 실행된 이후에는 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리가 모두 실행되고 실행된 쿼리는 제거된다. 따라서 데이틀에 데이터가 저장된다.

```
Hibernate: drop table if exists member CASCADE 
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, email varchar(255), primary key (member_id))
Hibernate: call next value for hibernate_sequence
Hibernate: call next value for hibernate_sequence

Hibernate: insert into member (email, member_id) values (?, ?)
Hibernate: insert into member (email, member_id) values (?, ?)
```

- 아래 2개의 로그를 보면 쓰기 지연 SQL 저장소에 저장된 INSERT 쿼리가 실행된 것을 볼 수 있다.

### **✔️ 영속성 컨텍스트와 테이블에 엔티티 업데이트**

```java
package com.codestates;

import com.codestates.member.entity.Member;
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;

/**
 * @Configuration 애너테이션을 추가하면 Spring에서 Bean 검색 대상인 Configuration 클래스로 간주해서
 * @Bean 애너테이션이 추가된 메서드를 검색한 후, 해당 메서드에서 리턴하는 객체를 Spring Bean으로 추가해준다.
 */
@Configuration
public class JpaBasicConfig {
    /**
     * JPA의 영속성 컨텍스트를 관리하는 클래스이다.
     * EntityManager 클래스의 객체는 EntityMangerFactory 객체를 Spring으로부터 DI 받을 수 있다.
      */
    private EntityManager em;

    //
    private EntityTransaction tx;

    /**
     * CommandLineRunner 객체를 람다 표현식으로 정의해주면 애플리케이션 부트스트랩 과정이 완료된 후에
     * 람다 표현식에 정의한 코드를 실행해준다.
     */
    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();

        /**
         * EntityManager를 통해서 Transaction 객체를 얻는다.
         * JPA에서는 이 Transaction 객체를 기준으로 데이터베이스의 테이블 데이터를 저장한다.
         */
        this.tx = em.getTransaction();

        /**
         * EntityManageFactory의 createEntityManager()메서드를 이용해서 EntityManager 클래스의 객체를 얻을 수 있다.
         * 이제 이 EntityManager 클래스의 객체를 통해서 JPA의 API메서드를 사용할 수 있다.
         */
        return args -> {
            example04();
        };
    }

	  ...
    public void example04() {
        tx.begin();
        // member 객체를 영속성 컨텍스트의 1차 캐시에 저장한다.
        em.persist(new Member("gksmfcksqls@gmail.com"));
        // 영속성 컨텍스트의 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리문을 실행한다.
        tx.commit();

        tx.begin();
        /**
         * 저장된 member 객체를 영속성 컨텍스트의 1차 캐시에서 조회한다. (테이블에서 조회하는 것이 아님)
         * 영속성 컨텍스트의 1차 캐시에 이미 저장된 객체가 있기 때문에 영속성 컨텍스트에서 조회한다.
         */
        Member member1 = em.find(Member.class, 1L);
        /**
         * setter 메서드로 이메일 정보를 변경한다.
         * em.update() 같은 JPA API가 있을 것 같지만 setter 메서도르 값을 변경하기만 하면 업데이트 로직이 완성된다.
         */
        member1.setEmail("Ekdcksqls@gmail.com");
        // tx.commit()을 실행하면 쓰기 지연 SQL 저장소에 등록된 UPDATE 쿼리가 실행된다.
        tx.commit();
    }
}
```

```
Hibernate: drop table if exists member CASCADE 
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, email varchar(255), primary key (member_id))

Hibernate: call next value for hibernate_sequence
Hibernate: insert into member (email, member_id) values (?, ?)

Hibernate: update member set email=? where member_id=?
```

- 아래 로그를 보면 UPDATE 쿼리가 실행이 된것을 확인할 수 있다.

### UPDATE 쿼리가 실행이 되는 과정

- setter 메서드로 값을 변경만 해도 tx.commit() 시점에 UPDATE 쿼리가 실행되는 이유가 뭘까?
- 영속성 컨텍스트에 엔티티가 저장될 경우에는 저장되는 시점의 상태를 그대로 가지고 있는 스냅샷을 생성한다.
- 그 후 해당 엔티티의 값을 setter 메서드로 변경한 후, `tx.commit()` 을 하면 변경된 엔티티와 전에 이미 떠 놓은 스냅샷을 비교한 후, 변경된 값이 있으면 쓰기 지연 SQL 저장소에 UPDATE 쿼리를 등록하고 UPDATE 쿼리를 실행한다.

### **✔️ 영속성 컨텍스트와 테이블의 엔티티 삭제**

```java
package com.codestates;

import com.codestates.member.entity.Member;
import org.springframework.boot.CommandLineRunner;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;

/**
 * @Configuration 애너테이션을 추가하면 Spring에서 Bean 검색 대상인 Configuration 클래스로 간주해서
 * @Bean 애너테이션이 추가된 메서드를 검색한 후, 해당 메서드에서 리턴하는 객체를 Spring Bean으로 추가해준다.
 */
@Configuration
public class JpaBasicConfig {
    /**
     * JPA의 영속성 컨텍스트를 관리하는 클래스이다.
     * EntityManager 클래스의 객체는 EntityMangerFactory 객체를 Spring으로부터 DI 받을 수 있다.
      */
    private EntityManager em;

    //
    private EntityTransaction tx;

    /**
     * CommandLineRunner 객체를 람다 표현식으로 정의해주면 애플리케이션 부트스트랩 과정이 완료된 후에
     * 람다 표현식에 정의한 코드를 실행해준다.
     */
    @Bean
    public CommandLineRunner testJpaBasicRunner(EntityManagerFactory emFactory) {
        this.em = emFactory.createEntityManager();

        /**
         * EntityManager를 통해서 Transaction 객체를 얻는다.
         * JPA에서는 이 Transaction 객체를 기준으로 데이터베이스의 테이블 데이터를 저장한다.
         */
        this.tx = em.getTransaction();

        /**
         * EntityManageFactory의 createEntityManager()메서드를 이용해서 EntityManager 클래스의 객체를 얻을 수 있다.
         * 이제 이 EntityManager 클래스의 객체를 통해서 JPA의 API메서드를 사용할 수 있다.
         */
        return args -> {
            example05();
        };
    }

   ...

    private void example05() {
        tx.begin();
        // Member 클래스의 객체를 영속성 컨텍스트의 1차 캐시에 저장한다.
        em.persist(new Member("gksmfcksqls@gmail.com"));
        // tx.commit()을 호출해서 영속성 컨텍스트의 쓰기 지연 SQL 저장소에 등록된 INSERT 쿼리를 실행한다.
        tx.commit();

        tx.begin();
        // 테이블에 저장된 Member 클래스의 객체를 영속성 컨텍스트의 1차 캐시에서 조회한다.
        Member member = em.find(Member.class, 1L);
        // em.remove()를 통해 영속성 컨텍스트의 1차 캐시에 있는 엔티티 제거를 요청한다.
        em.remove(member);
        // tx.commit()을 실행하면 영속성 컨텍스트의 1차 캐시에 있는 엔티티를 제공하고 쓰기 지연 SQL 저장소에 등록된 DELETE 쿼리가 실행된다.
        tx.commit();
    }
}
```

- `EntityManager` 의 `flush()` API
    
    코드에 나오지는 않지만 `tx.commit()` 메서드가 호출되면 JPA 내부적으로 `em.flush()` 메서드가 호출되어 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영한다.
    

```
Hibernate: drop table if exists member CASCADE 
Hibernate: drop sequence if exists hibernate_sequence
Hibernate: create sequence hibernate_sequence start with 1 increment by 1
Hibernate: create table member (member_id bigint not null, email varchar(255), primary key (member_id))

Hibernate: call next value for hibernate_sequence
Hibernate: insert into member (email, member_id) values (?, ?)

Hibernate: delete from member where member_id=?
```

- 맨 아래 로그를 보면 delete 쿼리가 실행된 것을 확인할 수 있다.

## 핵심 포인트

- JPA는 Java 진영에서 사용하는 ORM 기술의 표준 사양이다.
- Hibernate ORM은 JPA에서 정의해둔 인터페이스를 구현한 구현체로써 JPA에서 지원하는 기능 이외에 Hibernate 자체적으로 사용할 수 있는 API 역시 지원하고 있다.
- JPA에서는 테이블과 매핑되는 엔티티 객체 정보를 영속성 컨텍스트(Persistence Context)에 보관해서 애플리케이션 내에서 오래 지속되도록 한다.
- 영속성 컨텍스트 관련 JPA API
    - `em.persist()` 를 사용해서 엔티티 객체를 영속성 컨텍스트에 저장할 수 있다.
    - 엔티티 객체의 setter 메서드를 사용해서 영속성 컨텍스트에 저장된 엔티티 객체의 정보를 업데이트할 수 있다.
    - `em.remove()` 를 사용해서 엔티티 객체를 영속성 컨텍스트에서 제거할 수 있다.
    - `em.flush()` 를 사용해서 영속성 컨텍스트의 변경 사항을 테이블에서 반영할 수 있다.
    - `tx.commit()` 을 호출하면 내부적으로 `em.flush()` 가 호출된다.