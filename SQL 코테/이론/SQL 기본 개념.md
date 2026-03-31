
# SELECT & FROM
값 추출
- `select *`: 모든 컬럼 가져오기
- `select name, price`: 특정 컬럼만 가져오기
- `AS`: 컬럼 이름에 별명 붙이기

# WHERE
필터링
- 비교 연산자: `=`, `!=` (또는 `<>`), `<`, `>`, `<=`, `>=`
- 논리 연산자: `AND`, `OR`, `NOT`
- 범위/목록: `BETWEEN A AND B`, `IN (값1, 값2, ...)`
- 패턴 매칭 (중요!): `LIKE '%문자열%'` (`%`는 앞뒤에 무엇이든 올 수 있다는 뜻)

# ORDER BY
정렬
- `ASC`: 오름차순 (기본값, 생략 가능)
- `DESC`: 내림차순 (큰 수부터, 최신순)

# DISTINCT 
중복 제거
- `SELECT DISTINCT 컬럼명 FROM 테이블명;`

# LIMIT
딱 몇개만 데이터 뽑기
- `SELECT * FROM 테이블명 LIMIT 숫자;

# IS NULL / IS NOT NULL
`NULL`은 `=` 연산자로 찾을 수 없어서 특별한 문법이 필요하다.
- 값이 없는 경우: `WHERE 컬럼명 IS NULL`
- 값이 있는 경우: `WHERE 컬럼명 IS NOT NULL`

# LIKE
특정 글자가 포함된 데이터를 찾을 때 쓴다.
- `LIKE '강%'`: '강'으로 시작하는 모든 것 (강남, 강동원 등)
- `LIKE '%구'`: '구'로 끝나는 모든 것 (관악구, 해운대구 등)
- `LIKE '%산%'`: '산'이 들어가는 모든 것 (부산, 설악산, 산기슭 등)

# GROUP BY
데이터를 끼리끼리 묶는 기능
집계 함수(`COUNT`, `SUM`, `AVG` 등)와 항상 같이 다닙니다.
`GROUP BY 묶을컬럼

# HAVING
그룹화된 결과를 필터링
항상 where -> group by -> having 순서로 작성해야 한다.

# CASE WHEN
특정 조건에 따라 값을 바꾸어 출력, if랑 비슷함
- `CASE WHEN 조건 THEN 결과 ELSE 나머지 END

# 날짜 
- `YEAR()`, `MONTH()`, `DAY()`: 날짜에서 연, 월, 일만 추출
- `DATE_FORMAT(날짜, '%Y-%m-%d')`: 날짜 포맷 변경
- `DATEDIFF(날짜1, 날짜2)`: 두 날짜 사이의 차이 계산

# INNER JOIN
두 테이블 모두에 데이터가 존재하는 경우만 합쳐서 보여준다.

```sql
SELECT U.NAME, O.PRICE 
FROM USERS U 
INNER JOIN ORDERS O ON U.ID = O.USER_ID;
```

# LEFT JOIN (RIGHT JOIN)
왼쪽 테이블의 데이터는 무조건 다 보여주고, 오른쪽 테이블에 매칭되는 게 있으면 붙이고 없으면 `NULL`로 표시합니다.

```sql
SELECT U.NAME, O.PRICE 
FROM USERS U 
LEFT JOIN ORDERS O ON U.ID = O.USER_ID;
```

# SELF JOIN
같은 테이블을 두 번 불러와서 조인하는 기법이다. 보통 카테고리-상위 카테고리 관계나 직원-상급자 관계처럼 계층 구조를 가진 데이터를 다룰 때 쓴다.

```sql
SELECT E.NAME AS '직원', M.NAME AS '상사'
FROM EMPLOYEES E
INNER JOIN EMPLOYEES M ON E.MANAGER_ID = M.ID;
```

# 서브쿼리
- WHERE 절 서브쿼리: 특정 조건에 맞는 리스트를 먼저 뽑을 때 (가장 많이 씀)
    - 예: `WHERE PRICE = (SELECT MAX(PRICE) FROM PRODUCTS)` (가장 비싼 상품 찾기)    
- FROM 절 서브쿼리 (인라인 뷰): 테이블 자체를 가공해서 새로운 가상 테이블로 쓸 때
    - 예: `FROM (SELECT ... ) AS 가상테이블**`


# 집합 연산자 (UNION / UNION ALL)
# EXISTS / NOT EXISTS

