# 1. 데이터 구조 = table

| 이름  | 번호  | 주소  |
|:---:|:---:| --- |
| A   | -   | -   |
| B   |     |     |
| C   |     |     |

* **행(row) : 데이터 한 개를 의미** - `A - -`

* **열(column) : 데이터의 특성을 표현** - `이름`

# 2. 원하는 데이터 불러오기

**✔ `SELECT`**

```sql
SELECT * FROM customers
```

**`SELECT` : 가져오기**

**`*` - 전체**

**`FROM <table명>` : table로부터**

* 필요한 컬럼만 추출 가능
  
  ```sql
  SELECT city, state FROM station
  ```
  
  * 코드의 가독성을 위해 문법은 대문자, 나머지는 소문자로 작성

* 샘플 데이터를 조회할 때
  
  ```sql
  SELECT * FROM customers LIMIT 10
  ```
  
  **`LIMIT n` - 상위 n개 항목만**

**✔ `SELECT DISTINCT` - 컬럼에 있는 값들 중에 중복으로 있는 값을 한 번씩만 출력!**

```sql
SELECT DISTINCT city FROM station
WHERE city LIKE 'a%'
OR city LIKE 'e%'
OR city LIKE 'i%'
OR city LIKE 'o%'
OR city LIKE 'u%'
```

# 3. 조건에 맞는 데이터 검색

## ▶️ **비교연산자 (`=`, `<`, `>`, `>=`, `<=`)**

* **특정 컬럼이 특정 값을 가지는 데이터만 불러오기 위해서 사용!**

```sql
SELECT * FROM customers WHERE country = 'Germany'
SELECT * FROM customers WHERE customername < 'B' AND country = 'Germany'
```

**`customername < 'B'` : 손님 이름이 A로 시작하는 사람**

## ▶️ **논리연산자 (`AND`, `OR`)**

```sql
SELECT * FROM customers WHERE country = 'Germany' OR country = 'France'
-- 동일한 문장 
SELECT * FROM customers WHERE country IN ('Germany', 'France')
```

## ▶️ **`LIKE` : 문자열의 패턴을 찾는 역할**

> **일부분을 비교하는 부분 검색**

```sql
SELECT * FROM customers WHERE country LIKE 'Br%'
SELECT * FROM customers WHERE country LIKE 'B_____'
--언더바는 문자 수를 지정
SELECT * FROM customers WHERE country LIKE '%r%'
```

* **`LIKE 'Br%'` : 'Br'로 시작하는 단어를 찾는다.**

* **`LIKE '%r%'` : 앞뒤로 무슨 글자가 오든 중간에 'r'이 들어간 단어를 찾는다.**

* **`%`** : 무엇이든 상관없음을 의미

* **`_`** : 문자 수를 지정하여 찾는다.

* **`=`** : 찾고 싶은 키워드가 명확할 때 사용! **(속도가 `LIKE`에 비해 빠르다!)**

* *데이터 내 `%` 나 `_`를 찾고 싶을 때는 어떻게?*
  
  * `\` (역슬래시)를 사용! = escape

```sql
SELECT * FROM customers
WHERE discount LIKE '__\%' --역슬래시를 사용 (두자릿수 %를 찾음!)
```

**✔ `NOT LIKE a%` : a로 시작하지 않는 ~**

```sql
SELECT DISTINCT city FROM station
WHERE city NOT LIKE 'a%'
AND city NOT LIKE 'e%'
AND city NOT LIKE 'i%'
AND city NOT LIKE 'o%'
AND city NOT LIKE 'u%'
AND city NOT LIKE '%a'
```

## ▶️ **`IN` - 결과에 포함시키고자 하는 값 목록을 지정**

```sql
SELECT * FROM customers
WHERE country IN ('Germany', 'France', 'Korea')
```

## ▶️ **`BETWEEN` - 특정 범위 내에 있는 행만 선택**

> **`AND` 연산자와 쌍을 이루며 시작값, 끝값을 포함**

```sql
SELECT * FROM customers
WHERE customerID BETWEEN 3 AND 5;

SELECT * FROM products
WHERE customername BETWEEN 'C' AND 'M';

SELECT * FROM orders
WHERE customerorder BETWEEN '2019-01-01' AND '2020-01-01';
```

## ▶️ **`IS NULL` - 테이블에 값이 없는 경우를 찾을 때** **(반대 - `IS NOT NULL`)**

```sql
SELECT * FROM customers
WHERE customerID IS NULL
```

# 4. 데이터 정렬

## ▶️ **`ORDER BY` : 데이터를 정렬한다.**

> **default = ASC(오름차순)**

```sql
SELECT * FROM customers
ORDER BY customerid DESC --내림차순--
```

* **순서 : SELECT ~ FROM ~ WHERE ~ ORDER BY ~**

```sql
SELECT name FROM employee
WHERE months < 10     --조건 1--
AND salary > 2000     --조건 2--
ORDER BY employee_id
```

* **데이터를 가져와서 보여줄 때만 순서 변경됨❗ = 원본 데이터는 변경 ❌**

## ▶️ **다중 정렬 - 정렬 기준이 2개 이상일 때 (`,`로 연결)**

```sql
/* ORDER BY ~ 조건 1, 조건 2 */

SELECT name FROM students
WHERE marks > 75
ORDER BY RIGHT(name, 3), id
```

# 5. 문자열 자르기

> **MySQL 기준**

**✔ `LEFT(컬럼명 또는 문자열, 문자열의 길이)`**

**✔ `RIGHT(컬럼명 또는 문자열, 문자열의 길이)`**

**✔ `SUBSTRING(컬럼명 또는 문자열, 시작 위치, 길이)`**

```sql
SELECT LEFT("20140323", 4) --2014--
SELECT RIGHT("20140323", 4) --0323--
SUBSTR("20140323", 1, 4)  --2014--
SUBSTR("20140323", 5)     --0323 (5에서 끝까지)
```

# 6. 소수점 처리

> **MySQL 기준**

**✔ `CEIL() - 올림`**

**✔ `FLOOR() - 내림`**

**✔ `ROUND() - 반올림`**

```sql
SELECT CEIL(5.5)         --6--
SELECT FLOOR(5.5)        --5--
SELECT ROUND(5.556901, 4)--5.5569--
```

# 7. 집계 함수

## ▶️ 데이터의 개수 세기 - COUNT()

* 테이블에 총 몇 개의 데이터가 있는지?
  
  **`SELECT COUNT(*) FROM <table_name>`**

* 특정 기준을 만족하는 데이터는 몇 개인지?
  
  **`SELECT COUNT(*) FROM <table_name> WHERE <column_name> >= 10`**
  
  **`SELECT COUNT(DISTINCT <column_name>) FROM <table_name>`**
  
  * 기준: 금액, 날짜, 성별 등

* *데이터에 `null` 이 있다면?*
  
  * **`COUNT(*)` 의 경우, `null` 값도 포함**
  
  * **`COUNT(<column_name>)` 의 경우, 특정 컬럼의 null 값은 제외**

## ▶️ 합계 & 평균 - SUM(), AVG()

* 총 매출액은 얼마인가?
  
  **`SELECT SUM(<column_name>) FROM <table_name>`**

* 평균 결제 금액은 얼마인가?
  
  **`SELECT AVG(<column_name>) FROM <table_name>`**

* *데이터에 `null`이 있다면?*
  
  * *null 이 정말 null 인지, 0으로 해석해야 하는지 주의해야 함*
  
  * **특정 컬럼의 SUM과 AVG인 경우, null 값을 제외하고 계산!**
  
  * null 값을 포함하여 계산하려면 나누는 개수를 `COUNT(*)` 해야 원하는 데이터를 구할 수 있음

## ▶️ 최소값 & 최대값 - MIN(), MAX()

* 결제 금액이 가장 작었던 것과 가장 컸던 금액을 출력?
  
  **`SELECT MIN(total_bill), MAX(total_bill) FROM <table_name>`**

# 8. 그룹별로 요약하기

## ▶️ GROUP BY

> **GROUP BY 를 진행하는 컬럼은 SELECT 절에 반드시 포함해야 함**

* 요일별로 매출액의 합계는?
  
  **`SELECT <요일 컬럼>, SUM(<매출액 컬럼>) FROM <테이블명> GROUP BY <요일 컬럼>`**

* 요일별로 매출액, 비용의 합계는?
  
  **`SELECT <요일 컬럼>, SUM(<매출액 컬럼>), SUM(<비용 컬럼>) FROM <테이블명> GROUP BY <요일 컬럼>`**

* 요일, 시간대별로 매출액의 합계는?
  
  **`SELECT <요일 컬럼>, <시간대별 컬럼> SUM(<매출액 컬럼>) FROM <테이블명> GROUP BY <요일 컬럼>, <시간대별 컬럼>`**

* GROUP BY 뒤에 쓰는 컬럼 순서는 큰 의미 없음 (보여지는 순서만 다를 뿐!)

## ▶️ HAVING

> **요약 정보 필터링 하기**

* **`HAVING` : GROUP BY 후 연산 결과물을 필터링**
  
  * GROUP BY 에서 조건문으로 생각!
  
  * 예) 요일별 매출액의 합계에서 특정 매출액 이상만 조회 가능

### 📍 WHERE vs HAVING

- **`WHERE` : 원본 데이터에서 특정 조건을 만족하는 데이터를 조회할 때 사용**
  
  - 위치: `GROUP BY` 앞

- **`HAVING` : `GROUP BY` 사용 시, 연산한 결과에서 특정 조건을 만족하는 데이터 조회할 때 사용**
  
  - 위치: `GROUP BY` 뒤

## ▶️ ORDER BY

> **요약 정보 정렬하기**

* **`ORDER BY` 문은 가장 마지막에 사용**
  
  * **`LIMIT` 와 함께 사용하면 최댓값 최솟값을 구할 수 있음**

### 📌 [참고] Alias

* **`SELECT` 문에서 정의한 `AS`는 `GROUP BY`, `HAVING`, `ORDER BY` 절에서 사용 가능**

 
