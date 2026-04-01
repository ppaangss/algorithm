# 개요
### [언어별 개발자 분류하기](https://school.programmers.co.kr/learn/courses/30/lessons/276036)
### 유형

- 비트
### 회고

보통 이제 어떤 결과값을 뽑아내놓고 그거랑 비교하면서 결과를 출력하고 싶은 경우가 많았다. 근데 그거를 이제는 CTE를 통해 해결할 수 있다.

스칼라 서브쿼리라는 것을 사용할 수 있다는 걸 알아두자.

비트의 성질을 자세히 공부하는게 도움이 될 수 도 있을 것 같다.

# 해결 과정

내 풀이의 흐름은 일단 모든 등급-비트의 테이블을 만들었다. 그래서 거기에 해당하는 스킬을 가진 사람을 출력해줬다.

근데 문제가 이제 중복이 생길 수 있었다. A등급을 가진 사람이 C등급을 가진 상태에서도 C등급 또한 출력이 같이 됐다.

그래서 그 문제는 높은 등급만을 출력할 수 있도록 MIN 집계함수로 해결했다.

일단 내 코드의 문제점은 예를들어 프론트엔드 기술이 여러개고, 파이썬의 기술을 가지고 있는 사람이 있을 경우 여러 행이 출력된다. 그러면 계산량이 늘어나 성능적으로 떨어질 가능성이 있다.

내가 아는 선에서 해결을 하다보니 이렇게 난잡하게 됐는데 사실 비트의 성질을 잘 알고 있었다면 쉽게 해결할 수 있는 문제였다. 

비트의 성질을 잘 활용하면 프론트엔드의 기술 비트 합, C#의 비트, Python의 비트를 테이블로 만들어 프론트엔드 + Python, C#, 프론트엔드 이렇게 비트 비교를 해줘 CASE WHEN으로 해결이 가능하다. 

프론트엔드 기술들의 비트의 합을 구해도 & 연산을 통해 0이상이기만 한다면 반드시 프론트엔드 기술 1개 이상은 존재하는 거이기 때문에 좋은 풀이이다.

# 풀이

내 풀이

```sql
WITH TMP AS (
    SELECT S1.CODE + S2.CODE AS 'CODES', 'A' AS 'GRADE'
    FROM SKILLCODES S1
    JOIN SKILLCODES S2
    WHERE (S1.CATEGORY = 'Front End' AND S2.NAME = 'Python')

    UNION 

    SELECT CODE AS 'CODES', 'B' AS 'GRADE'
    FROM SKILLCODES
    WHERE NAME = 'C#'

    UNION

    SELECT CODE AS 'CODES','C' AS 'GRADE'
    FROM SKILLCODES
    WHERE CATEGORY = 'Front End'
)


SELECT MIN(GRADE) AS GRADE, D.ID, D.EMAIL
FROM TMP T
JOIN DEVELOPERS D
WHERE T.CODES = (D.SKILL_CODE & T.CODES)
GROUP BY D.ID, D.EMAIL
ORDER BY GRADE,D.ID
;
```

비트의 성질을 잘 사용한 풀이

```sql
WITH SKILL_INFO AS (
    -- Front End 스킬 코드들의 합과 특정 언어 코드를 미리 준비
    SELECT 
        (SELECT SUM(CODE) FROM SKILLCODES WHERE CATEGORY = 'Front End') AS FE_SUM,
        (SELECT CODE FROM SKILLCODES WHERE NAME = 'Python') AS PY_CODE,
        (SELECT CODE FROM SKILLCODES WHERE NAME = 'C#') AS CS_CODE
)
SELECT 
    CASE 
        -- A: FE 합계와 비트 연산 결과가 있고, Python과도 비트 연산 결과가 있는 경우
        WHEN (SKILL_CODE & FE_SUM) AND (SKILL_CODE & PY_CODE) THEN 'A'
        -- B: C#과 비트 연산 결과가 있는 경우
        WHEN (SKILL_CODE & CS_CODE) THEN 'B'
        -- C: 그 외 FE 합계와 비트 연산 결과만 있는 경우
        WHEN (SKILL_CODE & FE_SUM) THEN 'C'
    END AS GRADE,
    ID, 
    EMAIL
FROM DEVELOPERS, SKILL_INFO
-- GRADE가 없는 사람은 결과에서 제외 (중요!)
HAVING GRADE IS NOT NULL
ORDER BY GRADE ASC, ID ASC;
```