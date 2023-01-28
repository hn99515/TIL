# 서비스 이용 패턴 분석 (분석 상황 시뮬레이션)

> **뉴스레터를 보내거나 푸시, 쿠폰 등의 알람을 보낼 때 어떤 데이터를 봐야 가장 많은 사람들이 열람할 수 있을까?**

* GA 데이터를 바탕으로 진행

## 1️⃣ 일별 세션 수

```sql
SELECT DAYNAME(event_timestamp_kst) dayname
     , DATE(event_timestamp_kst) dt
     , COUNT(DISTINCT user_pseudo_id, ga_session_id) sessions
FROM ga
WHERE event_timestamp_kst BETWEEN '2023-01-01 00:00:00' AND '2023-01-07 23:59:59'
GROUP BY dayname, dt
ORDER BY dt
```

## 2️⃣ 요일 별, 시간 별 세션 수

```sql
SELECT DATE(event_timestamp_kst) dt
     , HOUR(event_timestamp_kst) hr
     , COUNT(DISTINCT user_pseudo_id, ga_session_id) sessions
FROM ga
WHERE event_timestamp_kst BETWEEN '2023-01-01 00:00:00' AND '2023-01-07 23:59:59'
GROUP BY dt, hr
ORDER BY dt, hr
```

* 데이터 추출 후 피봇 테이블 만들어서 보기 좋게 차트 확인 가능
  
  * 날짜 별, 시간대 별 세션 수를 확인 가능
  
  * 값의 크기에 따라 색상을 넣어주면 더욱 눈에 띔!

* 결과에 따라 어떤 이유로 우리 서비스를 이용하는지 어느 정도 확인할 수 있음

## 3️⃣ 클릭을 포함한 세션 수

> **페이지만 본 것이 아니라 스크롤을 내려보거나, 클릭을 해 본 행동에 대한 것도 알 수 있음**

```sql
SELECT DATE(event_timestamp_kst) dt
     , HOUR(event_timestamp_kst) hr
     , COUNT(DISTINCT user_pseudo_id, ga_session_id) sessions
FROM ga
WHERE event_timestamp_kst BETWEEN '2023-01-01 00:00:00' AND '2023-01-07 23:59:59'
AND event_name LIKE '%click%'
GROUP BY dt, hr
ORDER BY dt, hr
```

* 맥락이 다른 데이터는 색상 표기할 때도 다른 색상으로 표기하는 것이 좋음
  
  * 결과에 따른 해석을 계속 확인해보려는 습관이 중요

## 분석 결과를 나름대로 추론하고 어떤 제안을 하면 좋을지 고민하는 습관이 중요함!
