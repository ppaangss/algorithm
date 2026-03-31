# CTE

쿼리 안에서 잠시 쓰는 임시 결과판

복잡한 서브쿼리를 이름표를 붙여서 깔끔하게 정리해두고, 마치 실제 테이블처럼 가져다 쓰는 방식이다.

### 기본문법

- `WITH` 절과 함께 사용된다.
- 비재귀적 CTE에 해당한다.

```sql
WITH 테이블명 AS (
    SELECT 컬럼1, 컬럼2 ...
    FROM 원본테이블
    WHERE 조건
)
SELECT * FROM 테이블명; -- 위에서 만든 임시 판을 사용함
```

### 예시: 부서별 평균 급여보다 많이 받는 직원만 뽑기

employees 테이블

![](files/Pasted%20image%2020260331150807.png)

```sql
WITH DeptAvg AS (
    SELECT dept, AVG(salary) as avg_sal
    FROM employees
    GROUP BY dept
)
SELECT e.name, e.dept, e.salary, d.avg_sal
FROM employees e
JOIN DeptAvg d ON e.dept = d.dept
WHERE e.salary > d.avg_sal;
```

DeptAvg 테이블을 출력해보면 다음과 같이 나온다.

![](files/Pasted%20image%2020260331150847.png)

이 테이블을 사용하여 결과를 출력한다.

![](files/Pasted%20image%2020260331150937.png)
### 주의사항

1. 세미콜론(;)
	- CTE 바로 앞의 문장은 반드시 세미콜론으로 끝나야 한다.
2. 휘발성
	- CTE는 해당 쿼리가 실행되는 동안만 메모리에 존재한다. 쿼리가 끝나면 사라진다.
3. 여러 개 정의
	-  쉼표(,)로 구분해서 여러 개를 만들 수 있다.
4. 서브쿼리
	- 일반적인 CTE를 사용하는 경우는 모든 경우에 서브쿼리로 해결할 수 있다.
	- 단, 재귀 CTE의 경우는 서브쿼리로 해결할 수 없다.
	- 똑같은 서브쿼리를 여러번 사용해야 하는 경우에 CTE를 매우 유용하게 사용할 수 있다.
	- 서브쿼리보다 가독성이 좋아 사용하기 편하다.

```sql
    WITH CTE1 AS (...),
         CTE2 AS (...)
    SELECT * FROM CTE1 JOIN CTE2 ...
```

# 재귀 CTE

데이터를 스스로 생성하거나 계층 구조(조직도, 가계도 등)를 끝까지 파고들 때 사용한다.
### 기본 문법

- 재귀 CTE는 `WITH RECURSIVE` 절을 사용한다.
- UNION ALL을 사용해 두 부분을 연결한다.
- 종료조건 `WHERE` 을 반드시 필요로 한다.

```sql
WITH RECURSIVE 이름 AS (
    -- 1. 앵커(Anchor) 부분: 재귀의 시작점 (처음 딱 한 번만 실행)
    SELECT 초기값 AS 컬럼명
    
    UNION ALL
    
    -- 2. 재귀(Recursive) 부분: 자기 자신을 참조하여 반복 (조건이 맞을 때까지 실행)
    SELECT 반복계산식
    FROM 이름
    WHERE 반복중단조건
)
SELECT * FROM 이름;
```

### 예제: 시간대 0~23 테이블을 만들기

```sql
WITH RECURSIVE TimeTable AS (
    SELECT 0 AS hour
    UNION ALL
    SELECT hour + 1 FROM TimeTable WHERE hour < 23
)
SELECT *
FROM TimeTable T;
```

![](files/Pasted%20image%2020260331152104.png)

이런식으로 23까지 있는 테이블을 만들 수 있다.

### 예제: 주문을 시간대 별로 나누기

```sql
CREATE TABLE orders (
    order_id INT,
    order_time DATETIME
);

-- 일부러 중간중간 시간을 비워서 입력 (1시, 3시, 4시 등은 없음)
INSERT INTO orders VALUES 
(1, '2023-10-01 00:15:00'),
(2, '2023-10-01 00:45:00'),
(3, '2023-10-01 02:10:00'),
(4, '2023-10-01 05:20:00'),
(5, '2023-10-01 05:55:00');
```

![](files/Pasted%20image%2020260331152331.png)

이런 주문이 있다고 했을 때 이것을 시간대별로 나눌 것이다. 단, 없는 시간대는 0으로 표시해야 한다.

```sql
WITH RECURSIVE TimeTable AS (
    -- 0시부터 시작
    SELECT 0 AS hour
    UNION ALL
    -- 23시까지 1씩 증가하며 숫자 생성
    SELECT hour + 1 FROM TimeTable WHERE hour < 23
)
SELECT 
    T.hour, 
    COUNT(O.order_id) AS order_count
FROM TimeTable T
LEFT JOIN orders O ON T.hour = HOUR(O.order_time)
GROUP BY T.hour
ORDER BY T.hour;
```

![](files/Pasted%20image%2020260331152422.png)

해당 시간대의 개수만큼 늘어났고, 없는 시간대는 0으로 잘 나온다.

만약 재귀 CTE를 안쓰고 단순히 GROUP BY 만을 사용해 출력한다면 다음과 같은 결과가 나온다.

```sql
SELECT 
    HOUR(order_time) AS hour, 
    COUNT(*) AS order_count
FROM orders
GROUP BY HOUR(order_time)
ORDER BY hour;
```

![](files/Pasted%20image%2020260331152645.png)

이처럼 없는 값에 대한 시간대는 나오지 않는다. 

없는 시간대도 출력하고 싶으면 반드시 재귀 CTE를 사용해야 한다.

### 예제: 조직도 내 레벨 순위 찾기


