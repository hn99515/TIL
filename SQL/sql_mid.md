# 조건문 - CASE

> **if ~ else 문과 마찬가지로 특정 조건에 따른 행동을 분리할 수 있음**

* 사용법 : **`CASE WHEN ~ THEN ~ ELSE ~ END`**

```sql
SELECT CASE
            WHEN <조건1> THEN '행동1'
            WHEN <조건2> THEN '행동2' 
            ELSE '행동3'
       END AS <column_name>
FROM <table_name>
```

* 조건절에 비교 연산자, 논리 연산자가 들어갈 수 있다.

* 헷갈릴 때는 전체 테이블(`*`)를 같이 조회하면서 체크해보자!

* **CASE 문으로 만든 새로운 컬럼을 `GROUP BY`에도 사용하여 원하는 데이터 조회 가능**
  
  * 특정 조건에 맞는 데이터 조회가 가능해짐

* **조건의 순서에 따라 <mark>조건 포함 여부가 달라지므로 순서가 매우 중요</mark>함!**

* *`IF` 문은 조건을 1개만 설정할 수 있으며 `CASE WHEN` 은 조건을 여러개 설정 가능!*

# 테이블 피봇

> **행으로 나열되어 있던 데이터를 열 방향으로 변환해서 데이터를 한 눈에 볼 때 사용!**

* **우선, `CASE WHEN` 구문에서 집계함수 사용 가능**
  * 예)

```sql
SELECT AVG(CASE
            WHEN <조건1> THEN ''
            ELSE ''
       END) AS <new_column_name>
FROM <table_name>
```

* **세로(컬럼)로 데이터가 나오는 것을 가로로 피봇팅 가능!** = 열을 행으로 피봇팅
  
  * `CASE`를 활용하면 피봇팅 가능
  
  * `GROUP BY` 사용 시 집계함수를 이용하여 표현해야 함!
    
    * 예) id 별 + 조건에 맞게 피봇팅 하기

```sql
SELECT id
     , SUM(CASE WHEN <조건1> THEN <원하는 컬럼> ELSE NULL END) AS <new_column_name>
FROM <table_name>
GROUP BY id
```

* **각 컬럼별 총 합계를 쉽게 볼 수 있으며 데이터를 정리해서 볼 수 있다.**
  
  * **두 가지 기준으로 데이터를 확인하고 싶을 때는 피봇 테이블을 반드시 활용!!**
  
  * 첫 번째 열은 `GROUP BY` 로 컬럼을 정한다.

# DISTINCT

> **중복된 값을 하나로 표현하고 싶을 때 사용**

* *여러 컬럼일 때는 어떻게 적용되나?*
  
  * **컬럼 1, 컬럼 2의 조합(행을 기준)으로 하나씩 표현된다!**
  
  * 행이 다르면 데이터가 다른 값이므로 별도로 조회가 된다는 의미

```sql
SELECT DISTINCT <column1>, <column2>
FROM <table_name>
```

# JOIN

> **2개 이상의 테이블을 결합해서 사용하는 경우**

* 여러 테이블의 흩어져 있는 정보를 모아서 저장하고 싶은 경우
  
  * 예) 유저의 아이디, 연락처, 배송 주소, 구매한 상품의 이름, 상품의 가격, 구매한 개수 등을 확인할 때 한 테이블에 모두 저장할 경우 낭비되는 값이 너무 많이 발생
  
  * 구매한 상품의 이름, 상품의 가격, 구매한 개수 등은 주문정보 테이블을 별도로 생성하여 저장
  
  * **이후 필요할 때 `JOIN` 을 활용하여 정보 획득**
    
    * `Primay Key`를 기준으로 연결 가능

## ▶️ INNER JOIN

> **2개 이상의 테이블에서 교집합인 부분을 조회하고 싶은 경우**

* *구식 방법*
  
  * 예) 2개 테이블 전체의 경우의 수를 모두 구한 후 해당 데이터만 조회

```sql
SELECT *
FROM Customers, Orders
WHERE Customers.CustomerId = Orders.CustomerId
```

* **`JOIN` 사용 방법**
  
  * **예) 2개 테이블을 묶어줄 수 있는 컬럼을 `ON` 으로 지정**
  
  * 2개 이상의 테이블을 묶는 것 가능함 (연이어 `JOIN` 사용)

```sql
SELECT *
FROM Cutomers
    INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID
    INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
```

* *한 테이블의 NULL 값으로 구성되어 있는 경우, 조회된 결과에서 제외*

## ▶️ OUTER JOIN

> **INNER JOIN 을 제외한 모든 JOIN 은 OUTER JOIN!**

1️⃣ **`LEFT JOIN`**

* `JOIN`하고자 하는 테이블의 레코드가 NULL 값으로 구성되어 있어도 조회 가능
  
  * **예) 고객 정보는 있고, 주문 정보가 없는 경우도 함께 출력됨**

```sql
SELECT *
FROM Customers
    LEFT JOIN Orders ON Customers.CustomerId = Orders.CustomerId
```

* 예 2) 고객 정보는 있지만 주문 정보만 없는 레코드 조회

```sql
SELECT *
FROM Customers
    LEFT JOIN Orders ON Customers.CustomerId = Orders.CustomerId
WHERE OrderId IS NULL
```

2️⃣ **`RIGHT JOIN`**

* **`LEFT JOIN` 의 반대여서 보통은 `LEFT JOIN` 으로만 사용! (기준=FROM 테이블)**
  
  * 헷갈리기 쉬우므로 `LEFT JOIN`만 사용

# Self JOIN

> **자기 자신의 테이블끼리 JOIN**

```sql
SELECT *
FROM <table_name>
INNER JOIN <table_name> AS <tn> ON <table_name>.<column> = <tn>.<column_2>
WHERE <조건>
```

* 자기 자신과의 연결이 되는 테이블은 별칭 설정 및 기준 컬럼을 잘 연결시켜줘야 함
  
  * 날짜 데이터 컬럼끼리 연결해야 하는 경우 아래 참조

```sql
SELECT *
FROM <table_name> AS <tn>, <table_name> AS <tn2>
WHERE <조건>
```

* **서로 공통된 것이 없을 때는 FROM 절 뒤에 `,` 를 기준으로 나란히 작성**

# 날짜 데이터 연산

> **날짜형 데이터의 더하기, 빼기**

* **`DATE_ADD(기준날짜, INTERVAL)`**
  
  ```sql
  SELECT DATE_ADD(NOW(), INTERVAL 1 SECOND)
  SELECT DATE_ADD(NOW(), INTERVAL 1 MINUTE)
  SELECT DATE_ADD(NOW(), INTERVAL 1 HOUR)
  SELECT DATE_ADD(NOW(), INTERVAL 1 DAY)
  SELECT DATE_ADD(NOW(), INTERVAL 1 MONTH)
  SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR)
  SELECT DATE_ADD(NOW(), INTERVAL -1 YEAR)
  ```

* **`DATE_SUB(기준날짜, INTERVAL)`**
  
  ```sql
  SELECT DATE_SUM(NOW(), INTERVAL 1 SECOND)
  ```

# UNION

> **위, 아래로 데이터 이어붙이기**

* 같은 정보의 과거 테이블과 현재의 테이블을 이어 붙여야 할 때 유용
  
  * *중복된 요소는 하나로만 표현*

## ▶️ UNION ALL

* **중복된 요소가 있더라도 전부 다 표현**

```sql
SELECT *
FROM Products
WHERE price <= 5
OR price >= 200
```

```sql
SELECT *
FROM Products
WHERE price <= 5

UNION

SELECT *
FROM Products
WHERE price >= 200
```

* **회원의 주문 정보와 비회원의 주문 정보를 함께 조회하고자 할 때!** = 전체 정보 조회
  
  * **`LEFT JOIN` + `RIGHT JOIN` + `UNION` = `FULL OUTER JOIN`**
    
    * 단, MySQL 에서 `FULL OUTER JOIN` 은 사용 불가

```sql
SELECT *
FROM Customers
    LEFT JOIN Orders ON Customers.Id = Orders.CustomerId

UNION

SELECT *
FROM Customers
    RIGHT JOIN Orders ON Customers.Id = Orders.CustomerId
```

## 📌 [참고] 차집합 연산

> **ORACLE 에서는 MINUS, EXCEPT 를 지원**
> 
> **MySQL 은 지원 X**
