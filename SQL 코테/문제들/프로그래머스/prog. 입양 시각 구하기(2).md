# 개요
### [입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413)
### 유형

- 재귀 CTE
### 회고

`COUNT(*)` 를 쓰면 NULL이 있어도 행 자체가 있으면 있다고 간주한다. 따라서 ID가 있는 경우만 세야하므로 `COUNT(A.ANIMAL_ID)` 를 해준다.


# 해결 과정

나는 그냥 CTE 2개를 써서 간편하게 해결했다.

만약 재귀 CTE만 사용할거면 COUNT를 주의해야 한다.

`COUNT(*)` 를 쓰면 NULL이 있어도 행 자체가 있으면 있다고 간주한다. 따라서 ID가 있는 경우만 세야하므로 `COUNT(A.ANIMAL_ID)` 를 해준다.

# 풀이

### 내 풀이

```sql
WITH RECURSIVE HOUR_INFO AS(
    SELECT 0 AS 'HOUR'
    
    UNION ALL
    
    SELECT HOUR+1
    FROM HOUR_INFO
    WHERE HOUR < 23
),
COUNT_INFO AS(
    SELECT HOUR(DATETIME) AS 'HOUR', COUNT(*) AS 'COUNT'
    FROM ANIMAL_OUTS 
    GROUP BY HOUR(DATETIME)
    ORDER BY HOUR(DATETIME)
)
SELECT H.HOUR, IFNULL(C.COUNT,0) AS 'COUNT'
FROM COUNT_INFO C
RIGHT JOIN HOUR_INFO H ON C.HOUR = H.HOUR
; 
```

### 모범답안

```sql
WITH RECURSIVE HOUR_INFO AS (
    SELECT 0 AS HOUR
    UNION ALL
    SELECT HOUR + 1 
    FROM HOUR_INFO 
    WHERE HOUR < 23
)
SELECT 
    H.HOUR, 
    COUNT(A.ANIMAL_ID) AS COUNT -- IFNULL 대신 COUNT(컬럼명) 사용
FROM HOUR_INFO H
LEFT JOIN ANIMAL_OUTS A ON H.HOUR = HOUR(A.DATETIME)
GROUP BY H.HOUR
ORDER BY H.HOUR;
```