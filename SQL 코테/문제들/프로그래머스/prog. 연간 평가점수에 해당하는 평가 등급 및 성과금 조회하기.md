# 개요
### [연간 평가점수에 해당하는 평가 등급 및 성과금 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/284528)

### 유형

- GROUP BY
- CASE WHEN

### 회고

CASE WHEN 쓰는 방법 연습해야겠다.
# 해결과정

CTE로 평균 점수를 뽑아낸 다음에 CASE WHEN으로 평균점수 에 해당하는 등급과 성과금을 출력해주면 된다.

# 풀이

```sql
WITH TMP AS(
    SELECT EMP_NO, YEAR, AVG(SCORE) AS 'SC'
    FROM HR_GRADE G 
    GROUP BY EMP_NO, YEAR
)
SELECT E.EMP_NO, E.EMP_NAME, 
    CASE 
        WHEN T.SC >= 96 THEN 'S'
        WHEN T.SC >= 90 THEN 'A'
        WHEN T.SC >= 80 THEN 'B'
        ELSE 'C'
    END AS 'GRADE',
    CASE 
        WHEN T.SC >= 96 THEN (E.SAL * 0.2)
        WHEN T.SC >= 90 THEN (E.SAL * 0.15) 
        WHEN T.SC >= 80 THEN (E.SAL * 0.1) 
        ELSE 0
    END AS 'BONUS'
FROM HR_EMPLOYEES E
JOIN TMP T ON E.EMP_NO = T.EMP_NO
ORDER BY E.EMP_NO;

/*

사원별 성과금 정보
평가 점수별 등급


사번 성명 평가등급 성과금

*/
```