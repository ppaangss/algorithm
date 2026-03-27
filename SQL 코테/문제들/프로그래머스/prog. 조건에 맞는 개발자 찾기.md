https://school.programmers.co.kr/learn/courses/30/lessons/276034
# 회고

SQL에서도 비트마스킹을 할 수 있다.

# 풀이

```sql
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPERS
WHERE (SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'Python'))
   OR (SKILL_CODE & (SELECT CODE FROM SKILLCODES WHERE NAME = 'C#'))
ORDER BY ID ASC;
```