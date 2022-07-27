# 1. 데이터 구조 = table

| 이름  | 번호  | 주소  |
|:---:|:---:| --- |
| A   |     |     |
| B   |     |     |
| C   |     |     |

* 행(row) - 데이터 한 개를 의미 - A

* 열(column) - 데이터의 특성을 표현 - 이름

# 2. 보고 싶은 데이터 꺼내오기

✔ `SELECT`

```sql
SELECT * FROM customers
```

**`SELECT` - 가져오기**

**`*` - 전체**

**`FROM <table명>` - table로부터**

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

# 3. 조건에 맞는 데이터 검색하기

✔**비교연산자 (`=`, `<`, `>`, `>=`, `<=`)**

* **특정 컬럼이 특정 값을 가지는 데이터만 불러오기 위해서 사용!**

```sql
SELECT * FROM customers WHERE country = 'Germany'
SELECT * FROM customers WHERE customername < 'B' AND country = 'Germany'
```

**`customername < 'B'` - 손님 이름이 A로 시작하는 사람**

✔ **논리연산자 (`AND`, `OR`)**

**✔ `LIKE` - 문자열의 패턴을 찾는 역할 (일부분을 비교하는 부분 검색)**

```sql
SELECT * FROM customers WHERE country LIKE 'Br%'
SELECT * FROM customers WHERE country LIKE 'B_____'
--언더바는 문자 수를 지정
SELECT * FROM customers WHERE country LIKE '%r%'
```

**`LIKE 'Br%'` - 'Br'로 시작하는 단어를 찾는다.**

**`LIKE '%r%'` - 앞뒤로 무슨 글자가 오든 중간에 'r'이 들어간 단어를 찾는다.**

**`%`** - 무엇이든 상관없음을 의미

**`=`** - 찾고 싶은 키워드가 명확할 때 사용! **(속도가 `LIKE`에 비해 빠르다!)**

❓ **데이터 내 `%` 나 `_`를 찾고 싶을 때** ❓

```sql
SELECT * FROM customers
WHERE discount LIKE '__\%' --역슬래시를 사용 (두자릿수 %를 찾음!)
```

**✔ `NOT LIKE a%` - a로 시작하지 않는 ~**

```sql
SELECT DISTINCT city FROM station
WHERE city NOT LIKE 'a%'
AND city NOT LIKE 'e%'
AND city NOT LIKE 'i%'
AND city NOT LIKE 'o%'
AND city NOT LIKE 'u%'
AND city NOT LIKE '%a'
```

**✔ `IN` - 결과에 포함시키고자 하는 값 목록을 지정**

```sql
SELECT * FROM customers
WHERE country IN ('Germany', 'France', 'Korea')
```

**✔ `BETWEEN` - 특정 범위 내에 있는 행만 선택**

```sql
SELECT * FROM customers
WHERE customerID BETWEEN 3 AND 5;

SELECT * FROM products
WHERE customername BETWEEN 'C' AND 'M';

SELECT * FROM orders
WHERE customerorder BETWEEN '2019-01-01' AND '2020-01-01';
```

**✔`IS NULL` - 테이블에 값이 없는 경우를 찾을 때** (반대 - `IS NOT NULL`)

```sql
SELECT * FROM customers
WHERE customerID IS NULL
```


