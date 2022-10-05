# 목차
* [목차](#목차)
* [데이터베이스 관련 명령어](#데이터베이스-관련-명령어)
    + [데이터베이스 생성](#데이터베이스-생성)
	+ [데이터베이스 사용](#데이터베이스-사용)
    + [테이블 생성](#테이블-생성)
    + [테이블 정보 확인](#테이블-정보-확인)
* [SQL 명령어](#sql-명령어)
    + [SELECT](#select)
    + [FROM](#from)
    + [WHERE](#where)
    + [ORDER BY](#ordre-by)
    + [LIMIT](#limit)
    + [DISTINCT](#distinct)
    + [INNER JOIN](#inner-join)
    + [OUTER JOIN](#outer-join)
    + [여러 쿼리문을 한 번에 써보기](#여러-쿼리문을-한-번에-써보기)

# 데이터베이스 관련 명령어

## 데이터베이스 생성

```sql
CREATE DATABASE 데이터베이스_이름;
```

## 데이터베이스 사용

데이터베이스를 이용해 테이블을 만들거나 수정하거나 삭제하는 등의 작업을 하려면, 먼저 데이터 베이스를 사용하겠다는 명령을 전달해야 한다.

```sql
USE 데이터베이스_이름;
```

## 테이블 생성

`USE` 를 이용해 데이터베이스를 선택했다면, 이제 테이블을 만들 수 있다. 

다음은 user라는 테이블을 만드는 예제이다. 테이블은 필드(표의 열)와 함께 만들어야 한다. 다음과 같은 필드 조건이 있다고 가정하자

| 필드 이름 | 필드 타입 | 그 외의 속성 |
| --- | --- | --- |
| id | 숫자 | Primary key이면서 자동 증가되도록 설정 (이후에 천천히 배웁니다) |
| name | 문자열 (최대 255개의 문자) |  |
| email | 문자열 (최대 255개의 문자) |  |

```sql
CREATE TABLE user (
	id int PRIMARY KEY AUTO_INCREMENT,
	name varchar(255),
	email varchar(255)
);
```

위와 같이 입력하고, `DESCRIPBE` 명령어를 이용해 테이블 정보를 확인한다.

## 테이블 정보 확인

```sql
DESCRIBE user;
```

다음과 같은 정보를 확인할 수 있다.

```sql
mysql> describe user;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int          | NO   | PRI | NULL    | auto_increment |
| name  | varchar(255) | YES  |     | NULL    |                |
| email | varchar(255) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```

# SQL 명령어

## SELECT

- SELECT는 데이터셋에 포함될 특성을 특정한다.

```sql
// 문자열
SELECT 'hello world'
```

```sql
// 숫자
SELECT 2
```

```sql
// 간단한 연산
SELECT 15 + 3
```

## FROM

- 테이블과 관련한 작업을 할 경우 반드시 입력해야 한다. FROM 뒤에는 결과를 도출해낼 데이터베이스 테이블을 명시한다.

```sql
// 특정 특성을 테이블에서 사용
SELECT 특성_1
FROM 테이블_이름
```

```sql
// 몇 가지의 특성을 테이블에서 사용
SELECT 특성_1, 특성_2
FROM 테이블_이름
```

```sql
// 테이블의 모든 특성을 선택
SELECT *
FROM 테이블_이름
```

> *는 와일드카드(wildcard)로 전부 선택할 때에 사용된다.
> 

## WHERE

- 필터 역할을 하는 쿼리문이다. WHERE은 선택적으로 사용할 수 있다.

```sql
// 동일한 데이터 찾기
SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_1 = "특정 값"
```

```sql
// 특정 값을 제외한 값을 찾기
SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_2 <> "특정 값"
```

```sql
// 특정 값보다 크거나 작은 데이터를 필터할 때에는'<','>', 
// 비교하는 값을 포함하는 '이상','이하' 값은 '<=','>='을 사용한다.
SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_1 > "특정 값"

SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_1 <= "특정 값"
```

```sql
// 문자열에서 특정 값과 비슷한 값들을 필터할 때에는 'LIKE'와 '\%' 혹은 '\*'를 사용한다.
SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_2 LIKE "%특정 문자열%"
```

```sql
// 리스트의 값들과 일치하는 데이터들을 필터할 때에는 'IN'을 사용한다.
SELECT 특성_1, 특성_2
FROM 테이블_이름
WHERE 특성_2 IN ("특정값_1", "특정값_2")
```

```sql
// 값이 없는 경우 'NULL'을 찾을 때는 'IS'와 같이 사용한다.
SELECT *
FROM 테이블_이름
WHERE 특성_1 IS NULL
```

```sql
// 값이 없는 경우를 제외할 때에는 'NOT'을 추가해 이용한다.
SELECT *
FROM 테이블_이름
WHERE 특성_1 IS NOT NULL
```

## ORDRE BY

- 돌려받는 데이터 결과를 어떤 기준으로 정렬하여 출력할지 결정한다. ORDER BY는 선택적으로 사용할 수 있다.

```sql
// 기본 정렬은 오름차순이다.
SELECT *
FROM 테이블_이름
ORDER BY 특성_1
```

```sql
// 내림차순으로도 정렬할 수 있다.
SELECT *
FROM 테이블_이름
ORDER BY 특성_1 DESC
```

## LIMIT

- 결과로 출력할 데이터의 갯수를 정할 수 있다. LIMIT은 선택적으로 사용할 수 있다. 그리고 쿼리문에서 사용할 때에는 가장 마지막에 추가한다.

```sql
// 데이터 결과를 200개만 출력한다.
SELECT *
FROM 테이블_이름
LIMIT 200
```

## DISTINCT

- 유니크한 값을 받고 싶을 때에는 `SELECT DISTINCT` 를 사용할 수 있다.

```sql
// 특성_1을 기준으로 유니크한 값들만 선택한다.
SELECT DISTINCT 특성_1
FROM 테이블_이름
```

```sql
// 특성_1,2,3의 유니크한 '조합' 값들을 선택한다.
SELECT 
	DISTINCT
		특성_1
		, 특성_2
		, 특성_3
FROM 테이블_이름
```

## INNER JOIN

- `INNER JOIN` 이나 `JOIN` 으로 실행 할 수 있다.

```sql
// 둘 이상의 테이블을 서로 공통된 부분을 기준으로 연결한다.
SELECT *
FROM 테이블_1
JOIN 테이블_2 ON 테이블_1.특성_A = 테이블_2.특성_B
```

## OUTER JOIN

- `OUTER JOIN` 은 다양한 선택지가 있다.

```sql
// 'LIFE OUTER JOIN'으로 LEFT INCLUSIVE을 실행한다.
SELECT *
FROM 테이블_1
LEFT OUTER JOIN 테이블_2 ON 테이블_1.특성_A = 테이블_2.특성_B
```

```sql
// 'RIGHT OUTER JOIN'으로 RIGHT INCLUSIVE를 실행한다.
SELECT *
FROM 테이블_1
RIGHT OUTER JOIN 테이블_2 ON 테이블_1.특성_A = 테이블_2.특성_B
```

## 여러 쿼리문을 한 번에 써보기

> 같은 결과를 출력하는 서로 다른 쿼리문이 있을 수 있다. 같은 결과를 다른 방법으로 표현할 수 있다.
> 

```sql
// Brazil에서 온 고객을 도시별로 묶은 뒤에, 각 도시 수에 따라 내림차순 정렬한다. 
// 그리고 CustomerId에 따라 오름차순으로 정렬한 3개의 결과만 요청하는 예시이다.
SELECT c.CustomerID, c.FirstName, count(c.City) as 'City Count'
FROM customers AS c
JOIN employees AS e ON c.SupportREpId = e.EmployeeId
WHERE c.Country = 'Brazil'
GROUP BY c.City
ORDER BY 3 DESC, c.CustomerId ASC
LIMIT 3
```