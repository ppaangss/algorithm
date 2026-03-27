
- 중복을 허용하지 않는 컨테이너
- 내부적으로 해시 테이블(Hash Table) 사용
- 삽입 / 탐색 / 삭제 평균 O(1)
- 내부 자동정렬이 없다.
	- set은 내부 자동 정렬이 있다.
	- 정렬이 필요할 경우 set을 사용한다.

# 사용

## 기본문법

```c++
// 선언
unordered_set<int> s;

// 삽입
s.insert(10);
s.insert(20);
s.insert(10); // 중복 → 무시됨

// 존재 여부
if (s.find(10) != s.end()) {
    // 10 존재
}

// 존재 여부 (0 또는 1)
if (s.count(10)) {
    // 존재
}

// 삭제
s.erase(10);

// 크기
int n = s.size();

// 비우기
s.clear();

```