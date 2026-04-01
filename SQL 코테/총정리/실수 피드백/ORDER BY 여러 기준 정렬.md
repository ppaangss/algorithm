# , 를 이용해 여러 기준 정렬이 가능하다.

ORDER BY 에서 `컬럼 정렬방법`을 `,` 기준으로 여러 개 가능하다.

```sql
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID ASC
```

