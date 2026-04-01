# 개요
### [자동차 대여 기록 별 대여 금액 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151141)

### 회고

날짜가 정해져 있으면 간단한 문제였다. 근데 나는 날짜가 정해져 있지 않는 문제라고 생각하고 풀었다.

일단 'N일 이상' 여기서 N을 추출해야 한다. 이건 ~로 해결하는 거 알아두자.

그리고 추출한 N은 String이다. 그래서 이걸 CAST로 정수로 형변환 해줘야 한다.

DATEDIFF 사용해서 날짜 차이를 계산할 수 있다.

# 해결 과정

날짜가 정해져 있지 않은 경우로 생각하고 풀었다.

날짜까지는 추출했는데 그다음 어떻게 해줘야 할지 몰랐다. 예를들어 50일 빌린 경우 30일과 7일 둘다 포함하는 행이 나오기 때문이다. 

그래서 이건 스칼라 서브쿼리를 이용해 30일과 7일 중에 큰 값을 선택해서 계산하도록 하면 되는 문제였다. 

# 풀이

### 날짜가 정해져 있는 경우

```sql
WITH RENTAL_INFO AS (
    -- 1. 트럭의 대여 기록과 대여 기간, 기간 유형을 정리
    SELECT 
        h.HISTORY_ID,
        c.DAILY_FEE,
        c.CAR_TYPE,
        DATEDIFF(h.END_DATE, h.START_DATE) + 1 AS DURATION,
        CASE 
            WHEN DATEDIFF(h.END_DATE, h.START_DATE) + 1 >= 90 THEN '90일 이상'
            WHEN DATEDIFF(h.END_DATE, h.START_DATE) + 1 >= 30 THEN '30일 이상'
            WHEN DATEDIFF(h.END_DATE, h.START_DATE) + 1 >= 7 THEN '7일 이상'
            ELSE NULL 
        END AS DURATION_TYPE
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY h
    JOIN CAR_RENTAL_COMPANY_CAR c ON h.CAR_ID = c.CAR_ID
    WHERE c.CAR_TYPE = '트럭'
)

-- 2. 할인 정책을 조인하여 최종 금액 계산
SELECT 
    r.HISTORY_ID,
    ROUND(r.DAILY_FEE * r.DURATION * (1 - IFNULL(p.DISCOUNT_RATE, 0) / 100)) AS FEE
FROM RENTAL_INFO r
LEFT JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN p 
    ON r.CAR_TYPE = p.CAR_TYPE AND r.DURATION_TYPE = p.DURATION_TYPE
ORDER BY FEE DESC, r.HISTORY_ID DESC;
```

### 날짜가 정해져 있지 않는 경우

```sql
WITH RENTAL_INFO AS (
    -- 대여 기록 기본 정보 계산
    SELECT 
        h.HISTORY_ID,
        c.DAILY_FEE,
        c.CAR_TYPE,
        DATEDIFF(h.END_DATE, h.START_DATE) + 1 AS DURATION
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY h
    JOIN CAR_RENTAL_COMPANY_CAR c ON h.CAR_ID = c.CAR_ID
    WHERE c.CAR_TYPE = '트럭'
),
PLAN_CONVERTED AS (
    -- '90일 이상' 문자열에서 숫자 90을 추출하여 비교 가능하게 변환
    -- CAST(REGEXP_REPLACE(DURATION_TYPE, '[^0-9]', '') AS UNSIGNED) 등의 방식 사용
    SELECT 
        CAR_TYPE,
        DISCOUNT_RATE,
        CAST(REPLACE(DURATION_TYPE, '일 이상', '') AS UNSIGNED) AS MIN_DURATION
    FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN
    WHERE CAR_TYPE = '트럭'
)

SELECT 
    r.HISTORY_ID,
    ROUND(r.DAILY_FEE * r.DURATION * (1 - IFNULL(
        (SELECT MAX(p.DISCOUNT_RATE) 
         FROM PLAN_CONVERTED p 
         WHERE r.CAR_TYPE = p.CAR_TYPE AND r.DURATION >= p.MIN_DURATION
        ), 0) / 100)) AS FEE
FROM RENTAL_INFO r
ORDER BY FEE DESC, r.HISTORY_ID DESC;
```
