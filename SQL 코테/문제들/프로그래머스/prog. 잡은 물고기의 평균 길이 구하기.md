# 개요
### [잡은 물고기의 평균 길이 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/293259)
### 유형

- NULL
- 집계함수 (AVG)
- ROUND

### 회고

집계함수가 NULL을 어떻게 처리하는지 알아볼 수 있는 대표적인 문제이다.

집계함수는 NULL을 제외하고 처리한다. 만약 NULL을 임의의 어떤 값으로 보고 계산하고 싶으면 IFNULL이나 COALESCE를 사용하면 된다.

`ROUND(-,2)` -> 20.22 이런식으로 2번째까지 나오는거다.
# 해결 과정

`AVG(LENGTH)` 하면 NULL 을 제외하고 평균을 구한다.

`AVG(IFNULL(LENGTH,10))` 하면 NULL을 10으로 바꾸고 평균을 구한다.

# 풀이

```sql
SELECT ROUND(AVG(IFNULL(LENGTH,10)),2) AS AVERAGE_LENGTH
FROM FISH_INFO;
```