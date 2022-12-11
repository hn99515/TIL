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


