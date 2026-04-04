
SQL에서 `NULL`은 '값이 없음'이 아니라 **'알 수 없음(Unknown)'**을 의미하기 때문에, 우리가 흔히 생각하는 이진 논리(참/거짓)를 완전히 무너뜨리곤 합니다. 질문하신 `RANK` 함수를 포함해 실무에서 뒤통수를 맞기 쉬운 주요 상황들을 정리해 드릴게요.

---

## 1. 윈도우 함수와 `NULL`: `RANK`, `DENSE_RANK`

결론부터 말씀드리면, **`NULL`도 하나의 값으로 취급되어 순위가 매겨집니다.** 하지만 데이터베이스마다 `NULL`을 꼴찌로 볼지, 일등으로 볼지가 다릅니다.

- **동일한 순위:** `NULL`이 여러 개 있다면, 이들은 모두 같은 순위를 가집니다.
    
- **정렬 순서에 따른 위치:**
    
    - **Oracle / PostgreSQL:** `ASC` 정렬 시 `NULL`을 가장 큰 값으로 보아 마지막에 둡니다. (`NULLS LAST`가 기본)
        
    - **MySQL / SQL Server:** `ASC` 정렬 시 `NULL`을 가장 작은 값으로 보아 가장 처음에 둡니다. (`NULLS FIRST`와 같은 효과)
        

|**이름**|**점수**|**RANK (MySQL 기준)**|**설명**|
|---|---|---|---|
|김철수|NULL|1|NULL을 가장 작은 값으로 판단|
|이영희|NULL|1|위와 동일하므로 공동 1위|
|박짱구|80|3|앞서 2명이 있으므로 3위부터 시작|

---

## 2. 3진 논리 (Three-Valued Logic)의 함정

가장 무서운 부분입니다. SQL의 논리는 `TRUE`, `FALSE` 외에 `UNKNOWN`이 존재합니다.

- **`NULL = NULL`은 거짓(UNKNOWN)입니다.**
    
    - `WHERE score = NULL`이라고 쓰면 아무 결과도 나오지 않습니다. 반드시 `IS NULL`을 써야 합니다.
        
- **`NOT IN`의 재앙:**
    
    - `WHERE id NOT IN (1, 2, NULL)` 이 조건은 **항상 결과가 0건**입니다.
        
    - 내부적으로 `id != 1 AND id != 2 AND id != NULL`로 풀리는데, `id != NULL`이 `UNKNOWN`이 되어 전체 조건이 결코 `TRUE`가 될 수 없기 때문입니다.
        

---

## 3. 집계 함수(Aggregate Functions)의 무시

`COUNT`, `SUM`, `AVG` 같은 함수들은 기본적으로 **`NULL`을 계산에서 제외**합니다.

- **`COUNT(*)` vs `COUNT(컬럼)`**
    
    - `COUNT(*)`: `NULL`을 포함한 모든 행의 개수를 셉니다.
        
    - `COUNT(score)`: `score`가 `NULL`인 행은 제외하고 숫자를 셉니다.
        
- **`AVG`의 함정:**
    
    - 데이터가 `[100, NULL, 0]`일 때, `AVG`는 `(100 + 0) / 2 = 50`이 됩니다. 만약 `NULL`을 0점으로 간주해야 한다면 결과가 완전히 왜곡되므로, `COALESCE(score, 0)` 등을 사용해 처리해야 합니다.
        

---

## 4. 정렬(ORDER BY)에서의 위치

앞서 윈도우 함수에서 언급했듯, 일반 `ORDER BY`에서도 `NULL`의 위치는 DB 엔진마다 다릅니다. 이를 명시적으로 제어하려면 다음 구문을 활용하세요.

SQL

```
-- NULL을 무조건 뒤로 보내고 싶을 때 (Standard SQL)
SELECT * FROM students ORDER BY score ASC NULLS LAST;

-- 지원하지 않는 DB(MySQL 등)에서는 편법 사용
SELECT * FROM students ORDER BY (score IS NULL) ASC, score ASC;
```

---

## 요약: NULL을 상대하는 생존 법칙

1. **비교할 때는 `=` 대신 `IS NULL`을 써라.**
    
2. **`NOT IN`을 쓸 때 서브쿼리에 `NULL`이 섞여 있는지 백만 번 확인해라.** (안전하게 `NOT EXISTS` 권장)
    
3. **산술 연산(`+`, `-`, `*`, `/`)에 `NULL`이 하나라도 끼면 결과는 무조건 `NULL`이다.**
    
    - `100 + NULL = NULL`
        
4. **평균이나 합계를 구할 때 `NULL`이 어떤 의미인지 정의하고 `COALESCE`나 `IFNULL`로 치환해라.**
    

`NULL`은 오류를 내지 않고 '조용히' 결과를 틀리게 만들기 때문에 무서운 녀석입니다. 항상 "여기에 NULL이 들어오면 어떻게 되지?"라고 자문하는 습관이 중요합니다!

혹시 특정 DB(Oracle, MySQL 등)를 주로 사용하시나요? 환경에 따라 세부 동작이 조금씩 달라서 더 구체적으로 알려드릴 수 있습니다.