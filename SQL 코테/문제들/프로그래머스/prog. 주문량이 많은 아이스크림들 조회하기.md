https://school.programmers.co.kr/learn/courses/30/lessons/133027

# 회고

JOIN을 활용해 구할 수 있는 문제였다.

이 문제에서 이제 산술연산과 집계함수를 잠시 헷갈렸었다..

# 풀이

헷갈렸던 풀이

```sql
-- 코드를 입력하세요
SELECT F.FLAVOR
FROM FIRST_HALF F
JOIN (
    SELECT FLAVOR, SUM(TOTAL_ORDER) AS "TOTAL"
    FROM JULY
    GROUP BY FLAVOR
) T ON F.FLAVOR = T.FLAVOR
GROUP BY F.FLAVOR
ORDER BY SUM(TOTAL_ORDER + TOTAL) DESC
LIMIT 3
# GROUP BY F.FLAVOR
;

```

올바른 풀이

```sql
-- 코드를 입력하세요
SELECT F.FLAVOR
FROM FIRST_HALF F
JOIN (
    SELECT FLAVOR, SUM(TOTAL_ORDER) AS "TOTAL"
    FROM JULY
    GROUP BY FLAVOR
) T ON F.FLAVOR = T.FLAVOR
ORDER BY TOTAL_ORDER + TOTAL DESC
LIMIT 3
# GROUP BY F.FLAVOR
;

```