# EXISTS와 NOT EXISTS

EXISTS란, "안에 데이터가 단 한 줄이라도 있어?"라고 물어보는 '존재 여부 확인용' 함수이다.

EXISTS의 핵심은 찾으면 바로 서브쿼리를 나온다. 만약 하나라도 존재하는 값이 있으면 서브쿼리를 나오고 `TRUE`를 반환하는 것이다. 반대로, 하나라도 존재하지 않았다면 서브쿼리를 나오고 `FALSE`를 반환한다.

NOT EXISTS의 경우 반대로 적용해주면 된다. `TRUE`로 나온 결과를 `FALSE`로 , `FALSE` 로 나온 결과를 `TRUE` 로 반환해준다.
### 기본 문법

```sql
SELECT *
FROM 테이블A AS T
WHERE EXISTS (
    SELECT 1            -- 사실 뭘 써도 상관없음 (보통 1이나 * 사용)
    FROM 테이블B AS B
    WHERE T.id = B.id   -- 메인 쿼리와 서브쿼리를 연결하는 조건
);
```

- 출력 컬럼에 어떤 것을 사용해도 상관없이 `TRUE`나 `FALSE` 가 반환된다.
	- 따라서 `1`과 `*` 편한것을 사용하면 된다.

### IN 과의 비교

IN의 경우에는 만족하는 모든 데이터를 다 뒤져서 가져오지만 EXISTS 만족하는 즉시 쿼리를 나오게 된다.

| 특징      | IN                        | EXISTS               |
| ----------- | ----------------------------- | ------------------------ |
| 기준      | 서브쿼리의 결과 리스트와 비교          | 서브쿼리의 조건 만족 여부 확인    |
| NULL 처리 | 서브쿼리에 `NULL`이 있으면 결과가 꼬일 수 있음 | `NULL`에 상관없이 논리적으로 작동    |
| 성능      | 리스트가 작을 때 유리                  | 서브쿼리 쪽 테이블이 클 때 압도적으로 유리 |

### 예제: 부하직원이 없는 사람만 뽑기

```sql
CREATE TABLE employees (
    id INT,
    name VARCHAR(50),
    manager_id INT -- 상사의 id를 가리킴
);

INSERT INTO employees VALUES 
(1, 'CEO', NULL),          -- 최상위자 (상사 없음)
(2, 'Sales_Manager', 1),   -- CEO의 부하
(3, 'Dev_Manager', 1),     -- CEO의 부하
(4, 'Sales_Staff_A', 2),   -- Sales_Manager의 부하
(5, 'Sales_Staff_B', 2),   -- Sales_Manager의 부하
(6, 'Dev_Staff_A', 3);     -- Dev_Manager의 부하

```

![](files/Pasted%20image%2020260331160714.png)

```sql
SELECT id, name
FROM employees AS M  -- M은 Manager 후보군
WHERE NOT EXISTS (
    SELECT 1
    FROM employees AS E  -- E는 부하직원 후보군
    WHERE E.manager_id = M.id
);
```

![](files/Pasted%20image%2020260331160619.png)

먼저 employees의 각 행 마다 조건이 만족하는지를 확인한다.

- 만약에 CEO의 경우 서브쿼리에서 만족하는 행이 하나라도 있으므로 `TRUE` 를 반환한다. 그러면 `NOT EXISTS` 로 인해 `FALSE` 가 되어 내보내지 않는다.
- 만약에 Sales_Staff_A의 경우 서브쿼리에서 만족하는 행이 없으므로 `FALSE` 를 반환한다. 그러면 그러면 `NOT EXISTS` 로 인해 `TRUE` 가 되어 내보낸다.