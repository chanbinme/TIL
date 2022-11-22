* [DelegatingPasswordEncoder](#delegatingpasswordencoder)
    + [DelegatingPasswordEncoder 도입되기 전 문제점](#delegatingpasswordencoder-도입되기-전-문제점)
    + [DelegatingPasswordEncoder의 장점](#delegatingpasswordencoder의-장점)
    + [DelegatingPasswordEncoder를 이용한 PasswordEncoder 생성](#delegatingpasswordencoder를-이용한-passwordencoder-생성)
    + [Custom DelegatingPasswordEncoder 생성](#custom-delegatingpasswordencoder-생성)
    + [DelegatingPasswordEncoder Test](#delegatingpasswordencoder-test)

# DelegatingPasswordEncoder

> `DelegatingPasswordEncoder` 는 Spring Security에서 지원하는 `PasswordEncoder` 구현 객체를 생성해주는 컴포넌트이다.
> 
- DelegatingPasswordEncoder를 통해 애플리케이션에서 사용할 PasswordEncoder를 결정하고, 결정된 PasswordEncoder로 사용자가 입력한 패스워드를 단방향으로 암호화 해준다.

## DelegatingPasswordEncoder 도입되기 전 문제점

- Spring Security 5.0 이전 버전에서는 Plain text 패스워드를 그대로 사용하는 `NoOpPasswordEncoder` 가 디폴트 PasswordEncoder로 고정되어 있었다.
- 하지만 아래와 같은 문제들 때문에 DelegatingPasswordEncoder를 도입해서 조금 더 유연한 구조로 PasswordEncoder를 사용할 수 있게 되었다.

### 패스워드 인코딩 방식을 마이그레이션하기 쉽지 않은 오래된 방식을 사용하고 있는 경우

- 패스워드 단방향 암호화에 사용되는 hash 알고리즘은 시간이 지나면서 보다 더 안전한 hash 알고리즘이 지속적으로 고안되고 있기 때문에 항상 고정된 암호화 방식을 사용하는 것은 바람직하지 않다.
- 보안에 취약한 오래된 방식의 암호화 방식을 고수하는 애플리케이션은 해커의 좋은 타겟이 될 수 있다.

### 스프링 시큐리티는 프레임워크이기 때문에 호환성을 보장하지 않는 업데이트를 자주 할 수 없다.

- 오래된 하위 버전의 기술이 언젠가 Deprecated 되는 것처럼 보안에 취약한 오래된 방식의 암호화 알고리즘 역시 언제까지 관리 대상이 되지는 않는다.

## DelegatingPasswordEncoder의 장점

- `DelegatingPasswordEncoder` 를 사용해 다양한 방식의 암호화 알고리즘을 적용할 수 있다.
- 따로 암호화 알고리즘을 특별히 지정하지 않으면 Spring Security에서 권장하는 최신 암호화 알고리즘을 사용하여 패스워드를 암호화 할 수 있도록 해준다.
- 패스워드 검증에 있어서 레거시 방식의 암호화 알고리즘으로 암호화 된 패스워드의 검증을 지원한다.
- Delegating이라는 표현에서도 DelegatingPasswordEncoder의 특징이 잘 드러나듯이 나중에 암호화 방식을 변경하고 싶다면 언제든지 암호화 방식을 변경할 수 있다.
    - 단, 기존에 암호회되어 저장된 패스워드에 대한 마이그레이션 작업이 진행되어야 한다.

## DelegatingPasswordEncoder를 이용한 PasswordEncoder 생성

```java
PasswordEncoder passwordEncoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
```

- `PasswordEncoderFactories.createDelegatingPasswordEncoder();` : `DelegatingPasswordEncoder` 의 객체를 생성하고, 내부적으로 DelegatingPasswordEncoder 가 다시 적절한 PasswordEncoder 객체를 생성한다.

## Custom DelegatingPasswordEncoder 생성

- Spring Security에서 지원하는 `PasswordEncoderFactories` 클래스를 이용하면 기본적으로 Spring Security에서 권장하는 PasswordEncoder를 사용할 수 있다.
- DelegatingPasswordEncoder로 직접 PasswordEncoder를 지정해서 CustomDelegatingPasswordEncoder를 사용할 수 있다.

```java
String idForEncode = "bcrypt";
Map encoders = new HashMap<>();
encoders.put(idForEncode, new BCryptPasswordEncoder());
encoders.put("noop", NoOpPasswordEncoder.getInstance());
encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
encoders.put("scrypt", new SCryptPasswordEncoder());
encoders.put("sha256", new StandardPasswordEncoder());

PasswordEncoder passwordEncoder = new DelegatingPasswordEncoder(idForEncode, encoders);
```

## DelegatingPasswordEncoder Test

```java
package com.codestates.password;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.*;
import org.springframework.security.crypto.scrypt.SCryptPasswordEncoder;

import java.util.HashMap;
import java.util.Map;

@SpringBootApplication
public class DelegatingTest {
    public static void main(String[] args) {
        String idForEncode = "bcrypt";
        Map encoders = new HashMap<>();
        encoders.put(idForEncode, new BCryptPasswordEncoder());
        encoders.put("noop", NoOpPasswordEncoder.getInstance());
        encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
        encoders.put("scrypt", new SCryptPasswordEncoder());
        encoders.put("sha256", new StandardPasswordEncoder());

        PasswordEncoder passwordEncoder = new DelegatingPasswordEncoder(idForEncode, encoders);
        String password1 = passwordEncoder.encode("hello");

        passwordEncoder = new DelegatingPasswordEncoder("noop", encoders);
        String password2 = passwordEncoder.encode("hello");

        passwordEncoder = new DelegatingPasswordEncoder("pbkdf2", encoders);
        String password3 = passwordEncoder.encode("hello");

        passwordEncoder = new DelegatingPasswordEncoder("scrypt", encoders);
        String password4 = passwordEncoder.encode("hello");

        passwordEncoder = new DelegatingPasswordEncoder("sha256", encoders);
        String password5 = passwordEncoder.encode("hello");

        System.out.println("bcrypt = " + password1);
        System.out.println("noop = " + password2);
        System.out.println("pbkdf2 = " + password3);
        System.out.println("scrypt = " + password4);
        System.out.println("sha256 = " + password5);

        System.out.println(passwordEncoder.matches("hello", password1));
        System.out.println(passwordEncoder.matches("hello", password2));
        System.out.println(passwordEncoder.matches("hello", password3));
        System.out.println(passwordEncoder.matches("hello", password4));
        System.out.println(passwordEncoder.matches("hello", password5));
    }
}
```

```groovy
dependencies {
		implementation 'org.bouncycastle:bcprov-jdk15to18:1.72'
}
```

### 실행 결과

```java
bcrypt = {bcrypt}$2a$10$Q/4ckppMZ7eD25/NA2OW7enWrn7Be6mANqsTBKeb156HR4eMp7dRK
noop = {noop}hello
pbkdf2 = {pbkdf2}3981786d53d42bee127cce78d56e4a2fdc02711b59f5dedbd937c798536562b7521e7e4ca5259c1b
scrypt = {scrypt}$e0801$NxX5FUOB0ffnZ470v5zfWrUCXidnVhil5Pxc97R5hZJPKAMCLnjxBIL8ouEAN1Q5NVbrU5AMoPCps6t29kyfNg==$VbrACACMLOj4hEQLIvhmwEPR2rlLIHYsWhHX9cqR7Ug=
sha256 = {sha256}6eb29b4b275e3c7ccb91396b386c731e518733494b6e6e4267332c19c384230918cee68d39d0a9b4

true
true
true
true
true
```

- `Map encoders` : 원하는 유형의 PasswordEncoder를 추가해서 DelegatingPasswordEncoder의 생성자로 넘겨주면 디폴트로 지정한(`idForEncode`)한 passwordEncoder를 사용할 수 있다.
- `BCryptPasswordEncoder` 로 암호화 할 경우,
    - `{bcrypt}$2a$10$dXJ3SW6G7P50lGmMkkmwe.20cQQubK3.HZWzG3YB1tlRy.fqvM/BG`
        - **PasswordEncoder** id는 “bcrypt”
        - **encodedPassword**는$2a$10$dXJ3SW6G7P50lGmMkkmwe.20cQQubK3.HZWzG3YB1tlRy.fqvM/BG” 이다.
- `Pbkdf2PasswordEncoder`로 암호화 할 경우,
    - `{pbkdf2}5d923b44a6d129f3ddf3e3c8d29412723dcbde72445e8ef6bf3b508fbf17fa4ed4d6b99ca763d8dc`
        - **PasswordEncoder** id는 “pbkdf2”
        - **encodedPassword**는 “5d923b44a6d129f3ddf3e3c8d29412723dcbde72445e8ef6bf3b508fbf17fa4ed4d6b99ca763d8dc”이다.
- `SCryptPasswordEncoder`로 암호화 할 경우,
    - `{scrypt}$e0801$8bWJaSu2IKSn9Z9kM+TPXfOc/9bdYSrN1oD9qfVThWEwdRTnO7re7Ei+fUZRJ68k9lTyuTeUp4of4g24hHnazw==$OAOec05+bXxvuu/1qZ6NUR+xQYvYv7BeL1QxwRpY5Pc=`
        - **PasswordEncoder** id는 “scrypt”
        - **encodedPassword**는 “$e0801$8bWJaSu2IKSn9Z9kM+TPXfOc/9bdYSrN1oD9qfVThWEwdRTnO7re7Ei+fUZRJ68k9lTyuTeUp4of4g24hHnazw==$OAOec05+bXxvuu/1qZ6NUR+xQYvYv7BeL1QxwRpY5Pc=”이다.
- `StandardPasswordEncoder`로 암호화 할 경우,
    - `{sha256}97cde38028ad898ebc02e690819fa220e88c62e0699403e94fff291cfffaf8410849f27605abcbc0`
        - **PasswordEncoder** id는 “sha256”
        - **encodedPassword**는 “97cde38028ad898ebc02e690819fa220e88c62e0699403e94fff291cfffaf8410849f27605abcbc0”이다.