https://school.programmers.co.kr/learn/courses/30/lessons/157339
# 회고

3개 테이블을 join 하는 문제였다. 침착하게 문제를 잘 읽고 풀어야 한다.

기간 필터링을 할 때 좀 어려웠다. 일단 문제는 2022-11-01 부터 2022-11-30 까지 대여기록이 없는 차량만을 뽑아내는 문제였다.

그래서 단순히 `END_DATE < '2022-11-01' OR START_DATE >= '2022-12-01'` 이렇게 짰는데 틀렸다. 이렇게 짜면 예를들어 만약 어떤 차가 `10월 1일 ~ 10월 15일`에도 빌렸고, `11월 1일 ~ 11월 15일`에도 빌렸다고 가정하면 10월 기록을 보고 "오! 11월 전이네? 합격!" 하고 실제로는 11월에 대여 중인데도 그 차를 리스트에 넣어버린다. 따라서 안된다.

정답은 `NOT EXISTS`나 `NOT IN`을 사용하여 해당 기간에 대여 중인 차량 ID를 제외해야 한다.

MYSQL의 경우 MYSQL이 엔진의 편의성을 위해 HAVING에서 별칭을 사용할 수 있다. WHERE에서는 별칭을 사용할 수 없다. 

Oracle이나 SQL Server 에서는 불가능하다. 별칭을 사용하지 못하므로 기존 식 그대로를 사용해야 한다.

그리고 서브쿼리를 쓸지 조인을 쓸지 고민을 하는 과정을 가졌으면 좋을 것 같다.



# 풀이

정답

```sql
SELECT 
    C.CAR_ID, 
    C.CAR_TYPE, 
    -- 산술 연산은 그대로 쓰셔도 무방합니다!
    ROUND(C.DAILY_FEE * 30 * (1 - P.DISCOUNT_RATE / 100)) AS FEE
FROM CAR_RENTAL_COMPANY_CAR C
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN P 
    ON C.CAR_TYPE = P.CAR_TYPE 
    AND P.DURATION_TYPE = '30일 이상'
WHERE C.CAR_TYPE IN ('세단', 'SUV')
  -- 11월 한 달 내내 빌릴 수 있어야 함 (겹치는 기록이 없어야 함)
  AND C.CAR_ID NOT IN (
      SELECT CAR_ID
      FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
      WHERE START_DATE <= '2022-11-30' AND END_DATE >= '2022-11-01'
  )
-- WHERE 절에서 Alias(FEE)를 못 쓰므로 HAVING이나 계산식을 직접 사용
HAVING FEE >= 500000 AND FEE < 2000000
ORDER BY FEE DESC, CAR_TYPE ASC, CAR_ID DESC;
```

오답

```sql
SELECT C.CAR_ID, C.CAR_TYPE, ROUND(daily_fee * (100-discount_rate) * 0.01 * 30) AS 'FEE'
FROM CAR_RENTAL_COMPANY_CAR C
JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY H ON C.CAR_ID = H.CAR_ID
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN P ON C.CAR_TYPE = P.CAR_TYPE
WHERE DURATION_TYPE = '30일 이상'
AND C.CAR_TYPE in ('세단' ,'SUV')
AND (daily_fee * (100-discount_rate) * 0.01 * 30) >= 500000 
AND (daily_fee * (100-discount_rate) * 0.01 * 30) <= 2000000
AND (end_date < '2022-11-01' OR START_DATE >= '2022-12-01')
GROUP BY C.CAR_ID, C.CAR_TYPE
ORDER BY FEE DESC, CAR_TYPE ASC, CAR_ID DESC
;

```
