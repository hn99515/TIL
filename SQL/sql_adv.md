# 서브쿼리

> 쿼리 안의 쿼리를 의미

- 사용 예시

```sql
SELECT col1, col2
FROM <table_name>
WHERE col3 IN (SELECT col3 FROM <table_name2> WHERE col4 = '조건')
```

* **서브쿼리 사용 시 주의 사항!**
  
  * *`GROUP BY` + 날짜 : 기간 내에 하루 데이터가 없으면 평균을 구할 때 날짜가 하루 빠짐!*
    
    * **`AVG` 함수 사용할 때는 조심해야한다!!!**

## WHERE 절 서브쿼리

- **단일행 서브쿼리 : 비교 연산자(`>`, `<`, `<=`, `>=`, `!=(<>)`)와 함께 사용!**
  - **서브쿼리의 결과값은 1개여야 실행 가능**

```sql
SELECT *
FROM <table_name>
WHERE col1 > (SELECT AVG(col1) FROM <table_name>)
```

- **다중행 서브쿼리 : `IN`, `NOT IN`과 함께 사용!**
  - **서브쿼리의 결과값은 컬럼 1개, 로우 N개**

```sql
SELECT *
FROM <table_name>
WHERE col1 IN (
  SELECT col1
  FROM <table_name>
  GROUP BY col1
  HAVING SUM(col3) >= 1500
)
```

- 다중컬럼 서브쿼리 : 서브쿼리의 결과값은 컬럼 M개, 로우 N개
  - 원하는 조건의 전체 테이블을 보고 싶을 때 사용

```sql
SELECT *
FROM <table_name>
WHERE (col1, col2) IN (
  SELECT col1
     , MAX(col2)
  FROM <table_name>
  GROUP BY col1
)
```

## FROM 절 서브쿼리

> **Inline View 라고도 하며, 별칭을 반드시 써야 함!**
> 테이블로 활용할 수 있음 (`JOIN` 활용 가능!)

```sql
SELECT AVG(col2)
FROM (
  SELECT col1
     , SUM(col3)
  FROM <table_name>
  GROUP BY col1
) AS '별칭'
```

- **`WITH` 사용법 : FROM 절 서브쿼리 시 WITH 문으로 표현하는 경우가 많음** (가독성 높음)
  - 전체에서 차지하는 비율, 부분 비율 등을 구할 때 자주 사용!
  - `WITH` 문을 여러 번 사용할 수도 있음
  - **`WITH` 문은 테이블처럼 사용할 수 있도록 쿼리를 적었을 때만 가능하다는 의미미여 별도로 테이블이 저장되는 건 아님**

```sql
WITH '별칭' AS (
  SELECT col1
     , SUM(col3)
  FROM <table_name>
  GROUP BY col1
)

SELECT *
FROM '별칭'
     INNER JOIN <table_name> ON '별칭'.col1 = <table_name>.col1
```

```sql
WITH 별칭 AS (
  ...
), 별칭 AS (
  ...
)
```

## SELECT 절 서브쿼리

> 단일행 서브쿼리만 사용 가능 (자주 사용하진 않음)

```sql
SELECT t1.col1
     , t1.col2
     , ROUND(t1.col2 * 100 / (SELECT SUM(col2) FROM <table_name> t2 WHERE t2.col1 = t1.col1), 2) pct
FROM <table_name> t1
ORDER BY t1.col2 DESC
```
