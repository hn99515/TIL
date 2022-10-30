# 그룹 함수

> 문제 출제 100% = Group 함수들

![](SQLD_5_assets/2022-10-12-23-05-45-image.png)

```sql
SELECT 성별, 연령대, count(회원코드)
FROM c_info
GROUP BY 성별, 연령대;
```

![](SQLD_5_assets/2022-10-12-23-06-43-image.png)

![](SQLD_5_assets/2022-10-12-23-10-40-image.png)

## ▶ ROLLUP()

* **부분합계와 전체합계 값을 보여준다.**

* **인수의 순서에 영향을 받는다.**

```sql
SELECT 성별, 연령, SUM(결제금액)
FROM 결제
GROUP BY ROLLUP(성별, 연령대)
ORDER BY 성별, 연령대;
```

![](SQLD_5_assets/2022-10-12-23-08-45-image.png)

![](SQLD_5_assets/2022-10-12-23-11-00-image.png)

## ▶ CUBE()

* **그룹화될 수 있는 모든 경우에 대해 생성**

```sql
SELECT 성별, 연령, SUM(결제금액)
FROM 결제
GROUP BY CUBE(성별, 연령대);
```

![](SQLD_5_assets/2022-10-12-23-13-00-image.png)

![](SQLD_5_assets/2022-10-12-23-13-25-image.png)

## ▶ GROUPING SETS()

```sql
SELECT 성별, 연령, SUM(결제금액)
FROM 결제
GROUP BY GROUPING SETS(성별, 연령대);
```

![](SQLD_5_assets/2022-10-12-23-15-32-image.png)

![](SQLD_5_assets/2022-10-12-23-16-00-image.png)

```sql
SELECT 성별, 연령, SUM(결제금액)
FROM 결제
GROUP BY GROUPING SETS((성별, 연령대), 거주지);
```

![](SQLD_5_assets/2022-10-12-23-16-59-image.png)

## ▶ GROUPING 함수

> **소계, 합계 등이 계산되면 1을 반환하고, 아니면 0을 반환한다.**

```sql
SELECT 성별, GROUPING(성별) g1, 연령대, GROUPING(연령대) g2, SUM(결제금액)
FROM 결제
GROUP BY ROLLUP(성별, 연령대)
ORDER BY 성별, 연령대;
```

![](SQLD_5_assets/2022-10-12-23-58-56-image.png)

### 📍 문제

Q. 위 상태에서 CASE WHEN 을 활용하여 전체 합계를 구분하고 싶다면?

A.

```sql
SELECT 성별, CASE WHEN GROUPING(성별)=1 THEN '전체합계' END AS g1,
       연령대, GROUPING(연령대) g2, SUM(결제금액)
FROM 결제
GROUP BY ROLLUP(성별, 연령대)
ORDER BY 성별, 연령대;
```

![](SQLD_5_assets/2022-10-13-00-03-09-image.png)

### 📍 문제

Q. 보기의 GROUP BY 구문과 동일한 SQL문을 고르시오

`GROUP BY CUBE(name, phone);`

1. `GROUP BY ROLLUP(name);`

2. `GROUP BY GROUPING SETS(name, phone, (name, phone), ());`

3. `GROUP BY name UNION ALL GROUP BY name UNION ALL GROUP BY (name, phone)`

A. **`2`**

* CUBE는 가능한 모든 조합을 의미
  
  * name 별, phone 별, name & phone 별, 전체 합계 (빈 튜플을 의미)

### 📍 문제

Q. 다음 중 가장 적절하지 않은 것을 고르시오

1. CUBE 함수의 경우에는 함수의 인자가 주어진 순서에 따라 결과가 달라지며, 계층 구조로 집계값을 반환한다.

2. ROLLUP, CUBE 등 그룹 함수에 의해 집계된 결과에서 그룹 대상 컬럼 값은 NULL로 출력된다.

3. ROLLUP, CUBE 등 그룹 함수에 의해 집계된 결과에서 집계 대상 컬럼 값은 NULL로 출력된다.

4. ROLLUP, CUBE, GROUPING SETS 모두 일반 그룹 함수로 동일한 결과를 추출할 수 있다.

5. ROLLUP 은 함수 내 인자의 순서에 따라 다른 결과를 반환하는 함수이다.
   
   ![](SQLD_5_assets/2022-10-13-00-12-44-image.png)

A. **`1, 3`**

* 1번은 ROLLUP의 설명이다.

* 3번 집계 대상 컬럼 값은 NULL로 출력되는 것이 아니다.

# JOIN

> 테이블 간의 결힙. 집합과 유사.

* 교집합

`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `OUTER JOIN`

* 합집합

`UNION(ALL)`

* 차집합

`MINUS(oracle) = EXCEPT(SQL Server)`

* 결합되는 대상 간의 일치 정도

`EQUI 조인` <> `non-EQUI 조인`

* 조건구 없는 `CROSS JOIN`

## ▶ JOIN 진행 순서

1. **테이블간 결합의 key가 되는 컬럼 찾기**

![](SQLD_5_assets/2022-10-30-23-51-36-image.png)

2. JOIN 방식에 따라 결과가 달라지기 때문에 key 컬럼값 간의 일치도 파악

3. `INNER JOIN`을 진행한 결과 (공통된 것만 반환)

![](SQLD_5_assets/2022-10-30-23-53-01-image.png)

```sql
SELECT A.*, B.연령
FROM GENDER A INNER JOIN AGE B
ON A.회원코드 = B.회원코드;
```

```sql
SELECT A.*, B.연령
FROM GENDER A, AGE B
WHERE A.회원코드 = B.회원코드;
```

* **위 2가지 모두, 동일한 결과가 나옴 = `,` 로 JOIN 대체 가능**

## ▶️ INNER JOIN

> JOIN 후 특정 행만 호출할 때

![](SQLD_5_assets/2022-10-30-23-55-36-image.png)

```sql
-- JOIN 을 사용한 경우 WHERE 로 조건 
SELECT A.*, B.연령
FROM GENDER A INNER JOIN AGE B
ON A.회원코드 = B.회원코드
WHERE A.성별 = 'F';
```

```sql
-- , 를 사용한 경우 AND 로 조건 
SELECT A.*, B.연령
FROM GENDER A, AGE B
WHERE A.회원코드 = B.회원코드
AND A.성별 = 'F';
```

* 3개 이상의 테이블을 JOIN 할 때

![](SQLD_5_assets/2022-10-30-23-57-32-image.png)

```sql
SELECT A.*, B.연령, C.생년
FROM GENDER A JOIN AGE B ON A.회원코드 = B.회원코드
              JOIN BIRTH C ON B.연령 = C.생년;
```

```sql
SELECT A.*, B.연령, C.생년
FROM GENDER A, AGE B, BIRTH C
WHERE A.회원코드 = B.회원코드 AND B.회원코드 = C.생년;
```

## ▶️ OUTER JOIN

> **중복 + 나머지 모두 다 가져올 때**

![](SQLD_5_assets/2022-10-30-23-59-27-image.png)

![](SQLD_5_assets/2022-10-30-23-59-36-image.png)

```sql
SELECT A.*, B.연령
FROM GENDER A FULL OUTER JOIN AGE B
ON A.회원코드 = B.회원코드;
```

* `,` 를 활용한 `OUTER JOIN`은 불가능
  * **why**❓ `JOIN`은 `(+)` 기호로 표현 가능하지만 항상 한 쪽만 표기 가능❗ **OUTER JOIN은 양쪽 모두의 `(+)` 기호가 들어가야 하므로 표현 불가능함**❗

## ▶️ LEFT JOIN

> **왼쪽 테이블 기준으로 데이터를 가져다 붙이는 것**

![](SQLD_5_assets/2022-10-31-00-02-18-image.png)

```sql
SELECT A.*, B.연령
FROM GENDER A LEFT JOIN AGE B
ON A.회원코드 = B.회원코드;
```

```sql
SELECT A.*, B.연령
FROM GENDER A, AGE B
WHERE A.회원코드(+) = B.회원코드;
```

![](SQLD_5_assets/2022-10-31-00-02-33-image.png)

## ▶️ RIGHT JOIN

> **LEFT JOIN의 반대** - 반대이기에 보통 하나로만 사용

```sql
SELECT B.회원코드, 성별, 연령 
FROM GENDER A RIGHT JOIN AGE B
ON A.회원코드 = B.회원코드;
```

```sql
SELECT B.회원코드, 성별, 연령 
FROM GENDER A, AGE B
WHERE A.회원코드 = B.회원코드(+)
```

### 📍 문제

Q. 고객들의 이름과 전화번호를 함께 조회하려고 한다. 올바른 SQL문은?

![](SQLD_5_assets/2022-10-13-00-14-43-image.png)

A.

```sql
SELECT A.*, B.phone
FROM c_info A, PHONE B ON A.회원코드 = B.회원코드;
```

1. **두 테이블에서 동일한 컬럼을 찾는다.**

2. **원하는 정보가 도출될 수 있도록 레코드를 식별할 수 있는 컬럼으로 결정**

3. **PK 혹은 FK 기준으로 테이블들이 JOIN 될 수 있게끔 테이블이 설계되는 경우가 많음**

### 📍 문제

Q. SQL 을 통해 조회된 결과값이 아래와 같을 때, 빈칸에 알맞은 JOIN을 쓰시오

![](SQLD_5_assets/2022-10-31-00-06-09-image.png)

A.

```sql
SELECT * FROM T1 LEFT JOIN T2
ON T1.c2 = T2.c2;
```

## ▶️ UNION, UNION ALL

> **동일한 컬럼 개수와 데이터타입을 가진 두 테이블을 위아래로 합쳐줌**

![](SQLD_5_assets/2022-10-31-00-08-23-image.png)

* **`UNION`의 경우, 중복된 레코드가 제거된 후 합침**

![](SQLD_5_assets/2022-10-31-00-08-40-image.png)

```sql
SELECT * FROM T1
UNION
SELECT * FROM T2;
```

* **`UNION ALL` 의 경우, 중복된 레코드가 제거되지 않고 전체 다 나옴**

![](SQLD_5_assets/2022-10-31-00-09-22-image.png)

```sql
SELECT * FROM T1
UNION ALL
SELECT * FROM T2;
```

### 📍 문제

Q. 다음의 보기가 설명하는 데이터 결합 방식이 무엇인지 기술하시오

1. 관련된 **두 테이블에 적어도 하나의 공통 속성이 있을 때 적용할 수 있는 결합 방식**
   
   = `JOIN`

2. 조회 대상이 되는 컬럼 수가 같고 각 컬럼의 데이터타입 등 속성이 동일할 때 적용할 수 있는 결합 방식이다. **레코드 간의 중복이 제거된 결과가 반환된다.**
   
   = `UNION`

### 📍 문제

Q. 주어진 데이터에 대해 SQL 문을 실행한 결과를 구하시오.

![](SQLD_5_assets/2022-10-31-00-10-51-image.png)

```sql
SELECT * FROM T1
UNION
SELECT * FROM T2;
```

* **중복 레코드를 제거한 후 전체 테이블 합치기(위 아래로, 수직결합!)**
  
  ![](SQLD_5_assets/2022-10-31-00-11-33-image.png)
  
  Q.

```sql
SELECT count(T1.*) FROM T1, T2
WHERE T1.c3 = T2.c3;
```

* **`INNER JOIN`이며 key가 c3이다. 두 테이블의 교집합을 찾자!**

* 정답은 `3`

![](SQLD_5_assets/2022-10-31-00-12-42-image.png)

## ▶️ MINUS

> **차집합으로서 공통된거는 날리고 잔여 데이터만 보고 싶을 때 사용**

![](SQLD_5_assets/2022-10-31-00-13-44-image.png)

```sql
SELECT * FROM A
MINUS
SELECT * FROM B;
```

![](SQLD_5_assets/2022-10-31-00-14-16-image.png)
