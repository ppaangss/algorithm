# 회고

문제적 사고 흐름이 굉장히 어렵고 잘 안된다.
어떤식으로 테이블을 짜고 해야할지를 모르겠다.

일단 가볍게 정리를 해봤다 사고흐름을 ..

이 문제에 적용하자면
- 목표: ID, 연도, 편차를 출력하자. 정렬은 연도와 편차 순으로! (1단계)
- 가공: "연도별 최대값"이 필요하네? 근데 결과는 모든 ID가 다 나와야 해. ➔ 아, `GROUP BY`로 합쳐버리면 ID가 사라지니까 `MAX() OVER (PARTITION BY 연도)`를 써야겠다! (2단계)
- 필터링: 딱히 버릴 데이터는 없네? 테이블도 하나(`ECOLI_DATA`)만 쓰면 되겠다. (3단계)

그리고 이제 윈도우 함수에 MAX가 있다는 것을 알았다.

# 풀이

**윈도우함수 풀이**

```sql
SELECT 
    YEAR(DIFFERENTIATION_DATE) AS 'YEAR',
    (MAX(SIZE_OF_COLONY) OVER(PARTITION BY YEAR(DIFFERENTIATION_DATE)) - SIZE_OF_COLONY) AS 'YEAR_DEV',
    ID
FROM ECOLI_DATA 
ORDER BY YEAR ASC, YEAR_DEV ASC;

```

**내가 무지성으로 해본 윈도우함수 풀이**

```sql
SELECT 
    YEAR, 
    (MAX_SIZE - SIZE_OF_COLONY) AS YEAR_DEV, -- SUB 대신 - 연산 사용
    ID
FROM (
    SELECT 
        ID, 
        SIZE_OF_COLONY, 
        YEAR(DIFFERENTIATION_DATE) AS YEAR, 
        MAX(SIZE_OF_COLONY) OVER (PARTITION BY YEAR(DIFFERENTIATION_DATE)) AS MAX_SIZE -- AS 위치 수정
    FROM ECOLI_DATA
) AS ED -- 서브쿼리 별칭(ED) 필수!
ORDER BY YEAR ASC, YEAR_DEV ASC;

```

