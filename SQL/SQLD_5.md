# 그룹 함수 & JOIN

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

### 📍 문제

Q. 고객들의 이름과 전화번호를 함께 조회하려고 한다. 올바른 SQL문은?

![](SQLD_5_assets/2022-10-13-00-14-43-image.png)

A. 

```sql
SELECT A.*, B.phone
FROM c_info AS A PHONE AS B ON A.회원코드 = B.회원코드;
```

1. 두 테이블에서 동일한 컬럼을 찾는다.

2. 원하는 정보가 도출될 수 있도록 레코드를 식별할 수 있는 컬럼으로 결정

3. PK 혹은 FK 기준으로 테이블들이 JOIN 될 수 있게끔 테이블이 설계되는 경우가 많음


