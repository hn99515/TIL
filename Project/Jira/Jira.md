# Jira

> **이슈를 관리하고 프로젝트 플래닝, 애자일하게 사용할 수 있도록 도와주는 프로그램**

## ▶ JQL

> Jira Query Language

* **Jira Issue를 구조적으로 검색하기 위해 제공하는 언어**

* SQL 과 비슷한 문법

* Jira의 각 필드들에 맞는 특수한 예약어를 제공

* **쌓인 Issue들을 재가공해 유의미한 데이터를 도출해 내는데 활용**

## ▶ JQL Operators

* `=`, `!=`, `>`, `>=`

* `in`, `not in`

* `~ (contains)`, `!~ (not contains)` : 포함 여부 확인

* `is empty`, `is not empty`, `is null`, `is not null` : 큰 차이 없음

## ▶ JQL Dates

* Relative Dates 기능
  
  * **`Current` (오늘)를 기준으로 -1d, 1d, -1w, 1w, -2w, 2w 등으로 표현 가능**

## ▶ JQL Functions

* `endOfDay()`, `startOfDay()`

* `endOfWeek()` : 토요일을 의미, `startOfWeek()` : 일요일을 의미

* `endOfMonth()`, `startOfMonth()`, `endOfYear()`, `startOfYear()`

* `currentUser()`  

## ▶ JQL 활용예시

> **`project = "프로젝트명" and assignee = currentUser()` : 현재 접속한 유저의 내역을 확인 가능**

**`project = "pjt" and updated > startOfWeek(1d) and updated < endOfWeek(-1d)` : 월요일 ~ 금요일까지 업데이트 된 내역 확인**

* filter share, dashboard, gadget, Agile board 활용 가능

* 
