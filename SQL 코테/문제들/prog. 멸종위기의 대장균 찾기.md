# 개요

### [멸종위기의 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301651)

### 유형

- 재귀 CTE

### 회고

재귀 CTE 연습하자. 조직도를 어떻게 재귀 CTE로 할 수 있을지 연습해야 한다.

자식이 없는 사람을 어떻게 구할지를 많이 고민을 했는데 어려웠다. 이런 문제들을 많이 풀어보는게 좋을 듯

EXISTS도 알아두면 좋을 것 같다.

# 해결 과정

일단 자기가 몇세대인지를 구하는 것은 재귀 CTE로 구현할 수 있다. 

자식이 없는 사람을 구하는 것은 이제 결국 PARENT_ID 에 ID 값이 없는 사람이 바로 자식이 없는 사람인 것이다. 따라서 `WHERE`로 필터링을 해주면 된다.

# 풀이

```sql
WITH RECURSIVE TMP AS(
    SELECT ID, PARENT_ID, 1 AS 'lv'
    FROM ECOLI_DATA
    WHERE PARENT_ID IS NULL
    
    UNION ALL
    
    SELECT E.ID, E.PARENT_ID, lv+1 AS 'lv'
    FROM ECOLI_DATA E
    JOIN TMP T ON E.PARENT_ID = T.ID
)
SELECT COUNT(*) as 'COUNT', lv AS 'GENERATION'
FROM TMP T
WHERE NOT EXISTS (
    SELECT * 
    FROM TMP M
    WHERE T.ID = M.PARENT_ID
)
GROUP BY lv
ORDER BY lv;

```