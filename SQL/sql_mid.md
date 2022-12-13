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

# 테이블 피봇

> **세로(컬럼)로 표현하던 것을 가로로 피봇팅 할 수 있음**

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
