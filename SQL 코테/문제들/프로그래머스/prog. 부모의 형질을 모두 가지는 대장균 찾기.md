# 개요
### [부모의 형질을 모두 가지는 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301647)
### 유형

- 셀프조인
- 비트마스킹

### 회고

셀프조인과 비트마스킹에 관한 문제였다. 잘 몰라서 공부해야 겠다.

# 풀이

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