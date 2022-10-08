# SELECT

### 📍 문제

Q. 다음과 같이 정의된 테이블에서 2000년도에 태어난 고객의 이름과 전화번호를 조회하려고 한다. 빈칸에 알맞은 명령어를 구하라.

```sql
CREATE TABLE c_info (
    이름 varchar2(10),
    생년 number(4) default 9999,
    전화번호 varchar2(15) NOT NULL,
    첫방문일 date,
    고객번호 varchar2(10) primary key
);
```

A.  `SELECT  이름, 전화번호 FROM c_info WHERE 생년=2000;`

## ▶ 기본 구조

```sql
SELECT 컬럼명 등 FROM 테이블명
WHERE 조건문
GROUP BY 집계기준컬럼명
HAVING 그룹핑된 후 상태 기반의 조건문
ORDER BY 컬럼명
```

### 📍 문제

Q. 아래 테이블에 대해 아래와 같은 SQL문을 실행한 결과 출력되는 행의 수를 구하시오

이미지

`SELECT DISTINCT 성별, 연령대 FROM c_info`

A. 4개

 Why? 성별과 연령대 컬럼값을 통해 중복을 제거하고 조회한다.

### 📍 문제

Q. 아래 테이블에 대하여 SQL문 3가지를 실행한 결과, 출력되는 행 수를 구하라.

이미지

A.

`SELECT COUNT(*) FROM c_info` = 5

`SELECT COUNT(성별) FROM c_info` = 4 (NULL 값은 제외하고 count)

`SELECT COUNT(DISTINCT 성별) FROM c_info` = 3 (NULL 까지 구분하여 count)

## ▶ 문자형 함수

> SELECT 와 WHERE 문에 많이 사용되는 문자형 함수

* LOWER(문자열)
  
  * 영어 문자열 소문자로 변환
  
  * LOWER('SQL') `->` 'sql'

* UPPER(문자열)
  
  * 영어 문자열 대문자로 변환
  
  * UPPER('sql') `->` 'SQL'

* CONCAT(문자열1, 문자열2)
  
  * 문자열 1과 문자열 2를 결합
  
  * CONCAT('가', '나') `->` '가'||'나' (oracle)= '가'+'나'(Sqlserver)

* SUBSTR(문자열, m, n)
  
  * 문자열에서 m번째 자리값부터 n개를 자름
  
  * SUBSTR('KATE', 2, 2) `->` 'AT'

* LENGTH(문자열) = LEN(문자열)
  
  * 공백을 포함하여 문자열의 길이값을 반환
  
  * LEN('가 나다') `->` 4

* TRIM(문자열, 제거대상)
  
  * 왼쪽과 오른쪽에서 지정된 문자를 삭제
  
  * TRIM('  aabbccaa  ', ) `->` 'bbcc' (지정된 문자가 없으면 공백을 제거)
  
  * LTRIM(문자열, 제거대상) - 왼쪽에서, 지정된 문자를 삭제
  
  * RTRIM(문자열, 제거대상) - 오른쪽에서, 지정된 문자를 삭제

### 📍 문제

Q. 아래 왼쪽 테이블을 오른쪽과 같이 조회하기 위한 SQL문은?

이미지 삽입

A. `SELECT 회원코드, 연령대, UPPER(이름) FROM c_info`

이미지 삽입

A. `SELECT 회원코드, RTRIM(연령대, '대'), UPPER(이름) FROM c_info`

## ▶ 숫자형 함수

> SELECT 와 WHERE 문에 자주 사용되는 숫자형 함수

* ROUND(숫자, 소수점 자릿수)
  
  * 반올림
  
  * ROUND(25.3578, 2) `->` 25.36

* TRUNC(숫자, 소수점 자릿수)
  
  * 버림
  
  * TRUNC(25.3578, 2) `->` 25.35

* CEIL(숫자)
  
  * 크거나 같은 최소 정수 반환
  
  * CEIL(33.5) `->` 34

* FLOOR(숫자)
  
  * 작거나 같은 최소 정수 반환
  
  * FLOOR(33.5) `->` 33

* MOD(분자, 분모)
  
  * 분자를 분모로 나눈 나머지 반환
  
  * MOD(3, 2) `->` 1

* SIGN(숫자)
  
  * 숫자가 양수면 1, 0이면 0, 음수면 -1 반환

* ABS(숫자)
  
  * 절댓값

## ▶ 날짜형 함수

> SELECT 와 WHERE 문에 많이 사용되는 날짜형 함수

* SYSDATE
  
  * 쿼리를 돌리는 현재 날짜 & 시각을 출력
  
  * 2022/01/31 14:00:26 (datetime 형태)

* EXTRACT(정보 FROM 날짜)
  
  * 날짜형 데이터에서 원하는 값을 추출
  
  * EXTRACT (YEAR FROM date '2022-01-31') `->` 2022
    
    * 정보 - YEAR, MONTH, DAY, HOUR, MINUTE, SECOND
  
  * EXTRACT (YEAR FROM sysdate) TO_NUMBER(TO_CHAR(sysdate, 'YYYY'))
    
    * 위와 동일한 결과

## ▶ 명시적/암시적 형변환

> 명시적 형변환 - 형변환 함수를 사용하여 강제로 data type을 변경하는 것
> 
> 임시적 형변환 - 데이터베이스가 알아서 바꿔주는 것

* TO_NUMBER(문자열)
  
  * 문자열을 숫자로 변환
  
  * TO_NUMBER('2022')

* TO_CHAR(숫자 or 날짜, 포맷)
  
  * 숫자 혹은 날짜형 데이터를 포맷에 맞게 문자로 바꿈
  
  * TO_CHAR(date '2022-02-11', 'day') `->` '금요일'
  
  * TO_CHAR(200) `->` 200

* TO_DATE(문자열, 포맷)
  
  * TO_DATE('2022013120', 'YYYYMMDDHH24') `->` 2022/01/31 20:00:00

* 암시적 형변환 예시
  
  ```sql
  SELECT * FROM c_info WHERE 고객번호='12345'
  ```
  
  * 고객 번호가 number형으로 되어있고, PK값인 테이블에 대해 위와 같은 조회가 발생하면 암시적 형변환이 발생한다.
  
  * 여기서 고객번호는 숫자형 타입 + PK로 인덱스가 된다.
    
    * 인덱스? 빠른 조회를 돕는 기능을 함 (데이터는 인덱스 기준 자동 정렬됨)
    
    * 인덱스에 대해 암시적 형변환이 발생한 경우, 인덱스를 사용할 수 없다.

### 📍 문제

Q. 어제 날짜(YYYYMMDD)를 조회하기 위한 다음 SQL문의 빈칸을 채우시오

A. `SELECT TO_CHAR(SYSDATE-1, 'YYYYMMDD') FROM dual;`

* dual 테이블이란? 오라클에 존재하는 기본 테이블로, 하나의 열로만 이뤄져있음. 오늘 날짜를 구하거나 간단한 계산을 하는 등에 사용하기 적합함

## ▶ DECODE, CASE WHEN

> SELECT와 WHERE 문에서 자주 사용

* DECODE
  
  * IF 문
  
  *  `DECODE(값1, 값2, 값1=값2(참)일 때 출력값, 거짓일 때 출력값)``
  
  * DECODE(col1, KATE, '본인', '다른사람')

* CASE WHEN
  
  * 길어진 IF 문
  
  * `CASE WHEN 조건 THEN 조건이 참일 때 결과 ELSE 거짓일 때 결과 END`
  
  * CASE WHEN col1 < 10 THEN '한자리수' WHEN col1 BETWEEN 10 AND 99 THEN '두자리' ELSE '세자리수' END

### 📍 문제

Q. 아래의 SQL문을 실행했을 때 출력될 표를 기입하시오

```sql
SELECT 회원코드 AS c_id, 연령 AS AGE, 이름 AS NAME
FROM c_info
ORDER BY (CASE WHEN 회원코드=101 OR 회원코드=104 THEN 1 ELSE 2 END),
```


