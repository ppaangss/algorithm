# AND와 OR의 우선순위

prog.특정 형질을 가지는 대장균 찾기

일단 비트연산 4 또는 1 그리고 2 여야하는 문제였다.

그래서 다음과 같이 코드를 짰다.

```sql
SELECT count(*) AS "COUNT"
FROM ECOLI_DATA 
WHERE 1 = (GENOTYPE & 1)
OR 4 = (GENOTYPE & 4)
AND 0 = (GENOTYPE & 2);
```

AND와 OR 의 우선순위로 인해 AND가 먼저 수행된다.

```sql
SELECT count(*) AS "COUNT"
FROM ECOLI_DATA 
WHERE (1 = (GENOTYPE & 1)) 
OR ((4 = (GENOTYPE & 4))
AND (0 = (GENOTYPE & 2)) );
```

이런 뜻이다.

따라서 괄호를 쳐줘야 한다.

```sql
SELECT count(*) AS "COUNT"
FROM ECOLI_DATA 
WHERE ( (GENOTYPE & 1) OR (GENOTYPE & 4) ) -- 1번 또는 3번이 있음
AND (GENOTYPE & 2) = 0;                  -- 그리고 2번은 없음
```

https://school.programmers.co.kr/learn/courses/30/lessons/301646