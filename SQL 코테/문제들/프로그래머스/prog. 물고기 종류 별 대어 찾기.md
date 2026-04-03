# 개요
### [물고기 종류 별 대어 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/293261)
### 유형

- 서브쿼리
- 윈도우 함수
### 회고

도저히 말이 안되는 문제였다. 내가 아는 선에서는 WHERE로 1개의 컬럼만 받을 수 있는 줄 알았는데...

2개 이상을 받아서 비교할 수 도 있었다. 알아두자.

윈도우함수로도 연습해보자. 언젠다 써야할 수 도 있다.

# 해결과정

윈도우함수를 써서 풀어볼 수 도 있다. RANK()를 써서 풀 수 있고 그 외의 순위 함수가 있음.
# 풀이

**서브쿼리**

```sql
SELECT ID, FISH_NAME, LENGTH
FROM FISH_INFO I
JOIN FISH_NAME_INFO N ON I.FISH_TYPE = N.FISH_TYPE
WHERE (I.FISH_TYPE, I.LENGTH) IN (
    SELECT FISH_TYPE, MAX(LENGTH)
    FROM FISH_INFO
    GROUP BY FISH_TYPE
)
ORDER BY ID ASC;
```

**윈도우함수 사용**

```sql
SELECT ID, FISH_NAME, LENGTH
FROM (
    SELECT I.ID, N.FISH_NAME, I.LENGTH,
           RANK() OVER (PARTITION BY I.FISH_TYPE ORDER BY I.LENGTH DESC) AS rnk
    FROM FISH_INFO I
    JOIN FISH_NAME_INFO N ON I.FISH_TYPE = N.FISH_TYPE
) AS RankedFish
WHERE rnk = 1  -- 각 종류별로 1등만 뽑아라!
ORDER BY ID;
```