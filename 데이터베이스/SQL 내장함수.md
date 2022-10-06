# 목차
* [목차](#목차)
* [SQL 내장함수](#sql-내장함수)
    + [GROUP BY](#group-by)
	+ [HAVING](#having)
    + [COUNT()](#count)
    + [SUM()](#sum)
    + [AVG()](#avg)
    + [MIX(), MIN()](#mix-min)
* [SELECT 실행 순서](#select-실행-순서)

# SQL More

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1113fc75-a9f4-4764-9f19-339b49a5def9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221006T135319Z&X-Amz-Expires=86400&X-Amz-Signature=84e9e38b8757e1c4a34d17c15dab9e79fe1e8a6ae9160ad0aaecc03904396b41&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

위 이미지는 음원을 판매하는 사이트의 스키마 예시이다.

위 이미지의 스키마를 기준으로 이해해보자

SQL에서 사용되는 쿼리에는 유용하게 사용할 수 있는 함수가 많다. 그 중에서 자주 사용되는 몇 가지를 알아보자

## SQL 내장함수

### GROUP BY

데이터를 조회할 때 그룹으로 묶어서 조회한다. 다음과 같은 쿼리가 있다고 가정해보자

```sql
SELECT * FROM customers;
```

이 쿼리를 주(state)에 따라 그룹으로 묶어 표현할 수 있다.

```sql
SELECT * FROM customers
GROUP BY State;
```

GROUP BY쿼리로 간단하게 State에 따라 그룹화할 수 있다.  데이터베이스에서 데이터를 불러오는 과정에서 State에 따라 그룹을 지정했지만, 그룹에 대한 작업없이 조회만 했다. 그래서 쿼리의 결과로 나타나는 데이터는 각 그룹의 첫 번째 데이터만 표현된다.

## HAVING

HAVING은 GROUP BY 로 조회된 결과를 필터링 할 수 있다. 

```sql
// invoices 테이블을 CustomerID로 그룹화하고 
// 그 평균이 6을 초과한 결과를 조회
SELECT CustomerID, AVG(Total)
FROM invoices
GROUP BY CustomerID
HAVING AVG(Total) > 60
```

HAVING은 WHERE과는 적용하는 방식이 다르다. HAVING은 그룹화한 결과에 대한 필터이고, WHERE는 저장된 레코드를 필터링한다. 따라서 실제로 글부화 전에 데이터를 필터해야 한다면, WHERE을 사용한다.

## COUNT()

COUNT함수는 레코드의 갯수를 헤아릴 때 사용한다.

```sql
SELECT *, COUNT(*) FROM customers
GROUP BY State;
```

위 커맨드를 실행하면, 각 그룹의 첫 번째 레코드와 각 그룹의 레코드 갯수를 집계하여 리턴한다. 다음과 같이 변경하면, 그룹으로 묶인 결과의 레코드 갯수를 확인할 수 있다.

```sql
SELECT State, COUNT(*) FROM customers
GROUP BY State;
```

## SUM()

SUM 함수는 레코드의 합을 리턴한다. 

```sql
SELECT InvoiceId, SUM(UniPrice)
FROM invoice_items
GROUP BY InvoiceId;
```

위 커맨드는 invoice_items라는 테이블에서 InvoiceId필드를 기준으로 그룹하고, UniPrice필드 값의 합을 구한다.

## AVG()

AVG함수는 레코드의 평균값을 계산하는 함수이다.

```sql
SELECT TrackId, AVG(UniPrice)
FROM invoice_items
GROUP BY TrackId;
```

## MIX(), MIN()

Max와 MIn함수는 각각 레코드의 최대값과 최소값을 리턴한다. 

```sql
SELECT CustomerId, MIN(Total)
FROM invoices
GROUP BY CustomerId;
```

## SELECT 실행 순서

데이터를 조회하면 SELECT문은 정해진 순서대로 동작한다. 

1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY

```sql
SELECT CustomerID, AVG(Total)
FROM invoices
WHERE CustomerID >= 10
GROUP BY CutosmerID
HAVING SUM(Total) >= 30
ORDER BY 2;
```

이 쿼리문의 실행 순서는 다음과 같다.

1. `FROM invoices` : invoices 테이블에 접근한다.
2. `WHERE CustomerID >= 10` : CustomerID 필드가 10이상인 레코드들을 조회한다.
3. `GROUP BY CustomerID` : CustomerID를 기준으로 그룹화한다.
4. `HAVING SUM(Total) >= 30` : Total 필드의 총합이 30이상인 결과들만 필터링한다.
5. `SELECT CustomerID, AVG(Total)` : 조회한 결과에서 CustomerID 필드와 Total 필드의 평균값을 구한다.
6. `ORDER BY 2` : `AVG(Total)` 필드를 기준으로 오름차순 정렬한 결과를 2개 리턴한다.