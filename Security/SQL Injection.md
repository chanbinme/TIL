* [SQL Injection](#sql-injection)
    + [공격 시나리오](#공격-시나리오)
    + [세션기반 인증(Session-based Authentication)](#세션기반-인증session-based-authentication)
    + [SQL Injection 대응 방안](#sql-injection-대응-방안)


# SQL Injection

> SQL Injection(SQL 삽입)은 데이터베이스에 임의의 SQL문을 실행할 수 있도록 명령어를 삽입하는 공격 유형이다.
> 
- 응용 프로그램의 보안상의 허점을 이용해 데이터베이스를 비정상적으로 조작하며, 이로 인해 기록이 삭제되거나 데이터가 유출될 수 있다.
- SQL 삽입 공격은 보통 사용자가 input form에 직접 무언가 작성하는 상황에 발생한다.

## 공격 시나리오

1. 예를 들어 로그인을 한다면 웹 사이트에 로그인할 때, 입력한 아이디 값과 패스워드 값을 이용해 바로 데이터 베이스에 접근한다. 만약 클라이언트가 `kimchanbin` 이라는 아이디 값을 보낸다고 가정해보자

```sql
SELECT *
FROM users
WHERE auth='admin'
AND id='kimchanbin';
```

1. 공격자는 input form에 일반 텍스트(아이디 및 패스워드)가 아닌 SQL문을 작성한다. 입력받은 아이디와 패스워드를 통해 데이터베이스를 조회하는데, 패스워드에 `'' OR '1' = '1'` 을 넣어 보낸다면 다음과 같은 SQL 문이 완성된다.

```sql
SELECT *
FROM users
WHERE auth='admin'
AND id='' OR '1'='1';
```

1. `WHERE` 절에서 `OR` 는 `AND` 보다 연산 순위가 낮기 때문에 `OR` 절인 `'1' = '1'` (항상 참)이 가장 나중에 실행되어 결국 로그인에 성공하게 된다. 혹은 아래의 코드처럼 input form에 SQL문을 마무리하는 키워드인 `;` 와 함께 주요 테이블을 삭제하는 SQL문(e.g. `; DROP TABLES users;--'`)을 작성한다면 모두 삭제되는 피해를 입을 수도 있다.

```sql
SELECT * 
FROM users
WHERE auth='admin'
AND id='';DROP TABLES users;--';
```

## SQL Injection 대응 방안

### 1. 입력(요청) 값 검증

- SQL문은 사람이 사용하는 자연어와 비슷하기 때문에 키워드를 막기엔 한계가 있다.
- 따라서 블랙리스트가 아닌 화이트리스트 방식으로 해당 키워드가 들어오면 다른 값으로 치환하여 SQL Injection에 대응할 수 있다.

> 보안에서 화이트리스트란 기본 정책이 모두 차단인 상황에서 예외적으로 접근이 가능한 대상을 지정하는 방식 또는 그 지정된 대상들을 말한다.
> 

### 2. Prepared Statement 구문 사용

- Prepared Statement 구문을 사용하면 사용자의 입력이 SQL문으로부터 분리되어 SQL Injection을 방어할 수  있다.
- 사용자의 입력 값이 전달되기 전에 데이터베이스가 미리 컴파일하여 SQL을 바로 실행하지 않고 대기하며, 사용자의 입력값을 단순 텍스트로 인식한다.
- 따라서 입력값이 SQL문이 아닌 단순 텍스트로 적용되며 공격에 실패하게 된다.

### 3. Error Message 노출 금지

- 공격자는 데이터베이스의 Error Message를 통해 테이블이나 컬럼 등 데이터베이스의 정보를 얻을 수 있다.
- 에러가 발생한 SQL문과 에러 내용이 클라이언트에 노출되지 않도록 별도의 에러핸들링이 필요하다.