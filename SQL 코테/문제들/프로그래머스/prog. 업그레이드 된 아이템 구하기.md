https://school.programmers.co.kr/learn/courses/30/lessons/273711
# 회고

레벨2 치고 은근 까다로웠다. 조인과 서브쿼리 문제를 많이 풀어봐야 할 것 같다.

# 풀이

```sql
SELECT I.ITEM_ID, I.ITEM_NAME, I.RARITY
FROM ITEM_INFO I
JOIN ITEM_TREE T ON I.ITEM_ID = T.ITEM_ID
WHERE PARENT_ITEM_ID IN (
    SELECT ITEM_ID
    FROM ITEM_INFO
    WHERE RARITY = 'RARE')
ORDER BY I.ITEM_ID DESC;
```