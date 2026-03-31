# CASE WHEN

SQL 에서 if-else 문과 같은 역할을 한다. 특정 조건에 따라 다른 값을 반환하고 싶을 때 사용하며, 크게 두 가지 방식이 있다.

### 기본 문법

```sql
CASE 
    WHEN 조건1 THEN 결과1
    WHEN 조건2 THEN 결과2
    ELSE 기본결과
END
```

- `WHEN`: 참/거짓을 판단할 조건을 적습니다.
- `THEN`: 해당 조건이 참일 때 반환할 값을 적습니다.
- `ELSE`: 앞선 모든 조건이 거짓일 때 반환할 값을 적습니다. (생략 가능하며, 생략 시 `NULL` 반환)
- `END`: `CASE` 문의 끝을 알립니다.
### 예제 (단순 비교)

```sql
SELECT 이름,
    CASE 부서코드
        WHEN 'A' THEN '인사팀'
        WHEN 'B' THEN '개발팀'
        ELSE '기타'
    END AS 부서명
FROM 사원;
```

### 예제 (범위 및 복잡한 조건)

```sql
SELECT 이름, 점수,
    CASE 
        WHEN 점수 >= 90 THEN 'A학점'
        WHEN 점수 >= 80 THEN 'B학점'
        WHEN 점수 IS NULL THEN '미응시'
        ELSE 'C학점'
    END AS 등급
FROM 성적표;
```

