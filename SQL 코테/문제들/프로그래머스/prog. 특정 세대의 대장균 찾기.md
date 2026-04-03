# 개요

### [특정 세대의 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301650)
### 유형

- 재귀 CTE
### 회고

재귀 CTE만으로 풀리는 문제가 있다. 그니까 반드시 잘 알아놔야한다.

일반적으로 병합할 집합이 빈 집합이면 내부적으로 재귀가 멈추도록 설계되어있다.

이 문제에서는 LV 3까지만 구하면 되기 때문에 `WHERE` 절을 달아주는 것이 더 좋은 설계이다.

# 해결 과정

재귀 CTE를 통해 해결 가능하다.

# 풀이

```sql
WITH RECURSIVE TMP AS (
    SELECT ID, 1 AS 'LV'
    FROM ECOLI_DATA 
    WHERE PARENT_ID IS NULL
    
    UNION ALL
    
    SELECT E.ID, T.LV + 1 AS 'LV'
    FROM TMP T
    JOIN ECOLI_DATA E ON T.ID = E.PARENT_ID
)
SELECT ID
FROM TMP
WHERE LV = 3
ORDER BY ID;
```