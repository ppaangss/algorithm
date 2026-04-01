# HAVING에서 별칭을 사용하여 걸러주기

MYSQL 엔진은 HAVING 절을 처리할 때 SELECT 절에 정의된 별칭을 참조할 수 있게 설계되어 있다. 따라서 HAVING에서는 별칭을 사용할 수 있다.

단, Oracle이나 SQL Server(T-SQL) 같은 다른 DB에서는 `HAVING`에서 별칭을 쓰면 "부적합한 식별자"라며 에러가 난다.

만약 별칭을 사용하지 못하는 경우 기존 식 그대로를 사용하면 된다.

```sql
SELECT PRICE AS 'FEE'
FROM CAR
RE START_DATE <= '2022-11-30' AND END_DATE >= '2022-11-01'
HAVING FEE >= 500000 AND FEE < 2000000
```

