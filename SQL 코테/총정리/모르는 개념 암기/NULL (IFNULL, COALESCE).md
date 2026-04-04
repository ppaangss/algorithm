# IS NULL, IS NOT NULL

NULL 인지 아닌지 판단할 수 있는 함수이다.

`WHERE 컬럼명 IS NOT NULL` 

```sql
SELECT NAME
FROM ...
WHERE NAME IS NOT NULL;
```

# NULL 데이터 처리

NULL 인 값을 다른 값으로 바꾸기

- `IFNULL(컬럼명, '대체할 값')`
	- ex) PHONE 컬럼명 추출, NULL 이면 'NONE' 반환
		- `IFNULL(PHONE, 'NONE')`

- COALESCE 사용: NULL 이 아닌 것 반환
	- `COALESCE(값1, 값2, 값3 ... )`
	- 값1 이 NULL 이면 값2 확인, 값2가 NULL 이면 값3 확인
	- 앞에서 부터 확인해서 NULL이 아닌 값 반환
	- 전부 NULL 이면 NULL 반환
	- ex) PHONE 컬럼명 추출, NULL 이면 'NONE' 반환
		- `COALESCE(PHONE, 'NONE')`


```sql
SELECT IFNULL(PHONE, 'NONE')
FROM ...;
```

NULL 이면 NONE을 출력한다.

```sql
SELECT COALESCE(NAME,PHONE, 'NONE')
FROM ...;
```

NAME이 NULL 이면 PHONE 확인, PHONE NULL 이면 NONE 출력
NAME이 NULL 이면 PHONE 확인, PHONE NULL 이 아니면 PHONE 값 출력
NAME이 NULL 이 아니면 NAME 값 출력