* [목차](#목차)
* [JPA 연관 관계](#jpa-연관-관계)
    + [JPA에서 연관 관계](#jpa에서-연관-관계)
    + [연관 관계 정의 규칙](#연관-관계-정의-규칙)
    + [단방향, 양방향](#단방향-양방향)
    + [무조건 양방향 관계를 하면 쉬울까?](#무조건-양방향-관계를-하면-쉬울까)
    + [연관 관계의 주인](#연관-관계의-주인)
        + [왜 연관 관계의 주인을 지정해야 할까?](#왜-연관-관계의-주인을-지정해야-할까)
    + [다중성](#다중성)
        + [다대일(N:1)](#다대일n1)
        + [일대다(1:N)](#일대다1n)
        + [일대일(1:1](#일대일11)
        + [다대다(N:N)](#다대다nn)
        
# JPA 연관 관계

## JPA에서 연관 관계

> 객체와 관계형 데이터베이스 테이블이 어떻게 매핑되는지를 이해하는 것
> 

## 연관 관계 정의 규칙

연관 관계를 매핑할 때, 3가지 규칙을 고려해야 한다.

- 방향 : 단방향, 양방향 (객체 참조)
- 연관 관계의 주인 : 양방향일 때, 연관 관계에서 관리 주체
- 다중성 : 다대일(N:1), 일대일(1:1), 다대다(N:N)

## 단방향, 양방향

- 데이터베이스 테이블은 외래 키 하나로 양쪽 테이블 조인이 가능하다. 따라서 데이터베이스는 단방향, 양방향으로 나눌 필요가 없다.
- 하지만 객체는 참조용 필드가 있는 다른 객체를 참조하는 것이 가능하다.
- 두 객체 사이에 하나의 객체만 참조용 필드를 갖고 참조하면 단방향 관계
- 두 객체 모두가 각각 참조용 필드를 갖고 참조하면 양방향 관계라고 한다.
- 사실 양방향 관계는 두 객체가 단방향 참조를 각각 가져서 양방향 관계처럼 사용한다고 말하는 것이다.
- 비즈니스 로직에서 두 객체가 참조가 필요한지 여부를 고민해보면 된다.

## 무조건 양방향 관계를 하면 쉬울까?

- 양방향 매핑을 남용하면 엔티티 간의 불필요한 연관관계 매핑으로 인해 복잡성이 증가한다. 그렇기 때문에 양방향으로 할지 단방향으로 할지 구분해주어야 한다.
- 기본적으로 단방향 매핑으로 하고 나중에 역방향으로 객체 탐색이 꼭 필요하다고 느낄 때 추가하는 것이 좋다.

## 연관 관계의 주인

- 두 객체가 양방향 관계(단방향 관계 2개)를 맺을 때, 연관 관계의 주인을 지정해야 한다.
- 두 단방향 관계 중, 제어의 권한(외래 키를 비롯한 테이블 레코드를 저장, 수정, 삭제 처리)을 갖는 실질적인 관계가 어떤 것인지 JPA에게 알려준다고 생각하면 된다.
- 연관 관계의 주인은 두 객체 사이에서 조회, 수정, 삭제, 저장을 할 수 있지만, 연관 관계의 주인이 아니면 조회만 가능하다.
- 연관 관계의 주인이 아닌 개체에서 `mappedBy` 속성을 사용해서 주인을 지정해줘야 한다.
- **외래 키가 있는 곳을 연관 관계의 주인으로 지정해주자**

### 왜 연관 관계의 주인을 지정해야 할까?

- 예를 들어 두 객체(Board, Post)가 있고 양방향 연관 관계를 갖는다고 생각했을 때, 그 상황에서 게시글(Post)의 게시판을 다른 게시판(Board)으로 수정하려고 할 때, 어떤 객체의 메서드를 이용해서 게시글을 수정하는게 맞는지 헷갈릴 수 있다.
- 둘 다 맞는 방법이지만 양방향 연관 관계 관리 포인트가 두 개일 때 테이블과 매핑을 담당하는 JPA 입장에서 혼란을 주게된다.
- Post에서 Board를 수정할 때 FK를 수정할 지, Board에서 Post를 수정할 때 FK를 수정할 지를 결정하기 어려운 것이다.
- 그래서 연관 관계의 주인을 명확하게 해서 FK를 수정하겠다고 정하는 것이 좋다.

## 다중성

- 데이터베이스를 기준으로 다중성을 결정한다.
- 연관 관계는 대칭성을 갖는다.
    - 일대다 ↔ 다대일
    - 일대일 ↔ 일대일
    - 다대다 ↔ 다대다

### 다대일(N:1)

고객(Member)와 주문(Order)의 관계를 예로 들어보자

- 요구사항
    - 한 명의 고객(1)은 여러 개의 주문(N)을 할 수 있다.
    - 하나의 주문은 한 명의 고객만 받을 수 있다.
    - 주문과 고객은 다대일 관계를 갖는다.
- 데이터베이스는 무조건 다(N)쪽이 외래키를 갖는다.
- 외래 키는 게시글(N)이 관리한다.
- 다대일(N:1) 단방향
    
    ```java
    @Entity
    public class Order {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long orderId;
    
        @Enumerated(EnumType.STRING)
        private OrderStatus orderStatus = OrderStatus.ORDER_REQUEST;
    
        @ManyToOne
        @JoinColumn(name = "MEMBER_ID")
        private Member member;
    }
    
    @Entity
    public class Member {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long memberId;
    
        @Column(nullable = false, updatable = false, unique = true)
        private String email;
    
        @Column(length = 100, nullable = false)
        private String name;
    
        @Column(length = 13, nullable = false, unique = true)
        private String phone;
    ```
    
- 다대일(N:1) 양방향
    - 양방향으로 만드려면 일(1) 쪽에 `@OneToMany` 를 추가하고 `mappedBy` 애트리뷰트로 연관 관계의 주인을 지정해주면 된다.
    
    ```java
    @Entity
    public class Order {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long orderId;
    
        @Enumerated(EnumType.STRING)
        private OrderStatus orderStatus = OrderStatus.ORDER_REQUEST;
    
        @ManyToOne
        @JoinColumn(name = "MEMBER_ID")
        private Member member;
    }
    
    @Entity
    public class Member {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long memberId;
    
        @Column(nullable = false, updatable = false, unique = true)
        private String email;
    
        @Column(length = 100, nullable = false)
        private String name;
    
        @Column(length = 13, nullable = false, unique = true)
        private String phone;
    
        @OneToMany(mappedBy = "member")
        private List<Order> orders = new ArrayList<>();
    ```
    

### 일대다(1:N)

- 일대다는 연관관계의 주인을 일(1)쪽에 둔 것을 말한다.
- 일대다는 실무에서 거의 쓰지 않는다.
- 데이터베이스 입장에서 다(N)쪽에서 외래키를 관리하지만 일(1)쪽 객체에서 다(N) 쪽 객체를 조작(생성,저장,삭제)하는 방법이다.

### 일대일(1:1)

- 주 테이블에 외래키를 넣을 수도 있고, 대상 테이블에 외래키를 넣을 수도 있다.
- 게시글(Post)에 첨부파일(Attach)을 반드시 1개만 첨부할 수 있다고 가정해보자
- 일대일이라고 정할 때도 아주 신중하게 정한다고 가정하면 주 테이블에 외래 키를 두는 것이 더 낫다.

**일대일(1:1) 양방향**

```java
@Getter
@Setter
@Entity
public class Post {
    @Id @GeneratedValue
    @Column(name = "POST_ID")
    private Long id;

    @Column(name = "TITLE")
    private String title;
    @OneToOne
    @JoinColumn(name = "ATTACH_ID")
    private Attach attach;
}
@Getter
@Setter
@Entity
public class Attach {
    @Id @GeneratedValue
    @Column(name = "ATTACH_ID")
    private Long id;
    private String name;

		@OneToOne(mappedBy = "attach")
    private Post post;
}
```

### 다대다(N:N)

- 실무 사용 금지
- 다대다로 자동생성된 중간 테이블은 두 객체의 테이블의 외래 키만 저장되기때문에 문제가 될 확률이 높다.
- JPA는 중간 테이블에 외래 키 외에 다른 정보가 들어가는 경우가 많기 때문에 다대다를 일대다, 다대일로 풀어서 만드는 것(중간 테이블을 Entity로 만드는 것)이 추후 변경에 유연하게 대처할 수 있다.

```java
@Getter
@Setter
@Entity
public class OrderCoffee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long orderCoffeeId;

    @Column(nullable = false)
    private int quantity;

    @ManyToOne
    @JoinColumn(name = "ORDER_ID")
    private Order order;

    @ManyToOne
    @JoinColumn(name = "COFFEE_ID")
    private Coffee coffee;
}
```

```java
@Getter
@Setter
@Entity(name = "ORDERS")
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long orderId;

    @Enumerated(EnumType.STRING)
    private OrderStatus orderStatus = OrderStatus.ORDER_REQUEST;

    @OneToMany(mappedBy = "order", cascade = CascadeType.PERSIST)
    private List<OrderCoffee> orderCoffees = new ArrayList<>();
}
```

```java
@Getter
@Setter
@Entity
public class Coffee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long coffeeId;

    @Column(length = 100, nullable = false)
    private String korName;

    @Column(length = 100, nullable = false)
    private String engName;

    @Column(length = 5, nullable = false)
    private Integer price;

    @Column(length = 3, nullable = false, unique = true)
    private String coffeeCode;

    @OneToMany(mappedBy = "coffee")
    private List<OrderCoffee> orderCoffees = new ArrayList<>();
}
```

## 참고 자료

[https://jeong-pro.tistory.com/231](https://jeong-pro.tistory.com/231)