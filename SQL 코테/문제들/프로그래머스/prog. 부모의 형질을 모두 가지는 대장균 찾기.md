# 개요
### [부모의 형질을 모두 가지는 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301647)
### 유형

- 셀프조인
- 비트마스킹

### 회고

셀프조인과 비트마스킹에 관한 문제였다. 잘 몰라서 공부해야 겠다.

	2번째 풀었을 때 간단하게 CTE 써서 똑같은 풀이로 풀었다. 비트마스킹 조건을 어떻게 해야할지 고민을 했었는데 & 연산을 한 결과가 부모의 GENOTYPE과 같아야 하는게 조건이었다.

# 풀이
### 1차 풀이 

```sql
SELECT 
    CHILD.ID, 
    CHILD.GENOTYPE, 
    PARENT.GENOTYPE AS PARENT_GENOTYPE
FROM ECOLI_DATA AS CHILD
JOIN ECOLI_DATA AS PARENT 
    ON CHILD.PARENT_ID = PARENT.ID
WHERE (CHILD.GENOTYPE & PARENT.GENOTYPE) = PARENT.GENOTYPE
ORDER BY CHILD.ID ASC;

```

### 2차풀이 (CTE)

```sql
WITH GEN_INFO AS (
    SELECT E.ID, P.ID AS P_ID, E.GENOTYPE AS E_GEN, P.GENOTYPE AS P_GEN
    FROM ECOLI_DATA E
    JOIN ECOLI_DATA P ON E.PARENT_ID = P.ID
)
SELECT ID, E_GEN AS GENOTYPE, P_GEN AS PARENT_GENOTYPE
FROM GEN_INFO
WHERE (E_GEN & P_GEN) = P_GEN
ORDER BY ID
; 
```