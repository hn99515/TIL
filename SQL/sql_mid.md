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
