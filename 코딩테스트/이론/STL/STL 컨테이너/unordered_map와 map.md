
- Key → Value 형태로 데이터를 저장하는 자료구조
- 내부적으로 해시 테이블(Hash Table) 사용
- 탐색 / 삽입 / 삭제 평균 시간복잡도 O(1)
- 내부 자동정렬이 없다.
	- map은 내부 자동 정렬이 있다.
	- 정렬이 필요할 경우 map을 사용한다.

# 사용

## 기본 문법

```c++
unordered_map<string, int> m; // 선언

// "apple"이 없으면 자동 생성 (0)
m["apple"]++;

// 값 대입
m["banana"] = 3;

// ❌ 의도치 않게 apple이 생성될 수 있음.
if (m["apple"]) 

// 존재 여부 확인
if (m.find("apple") != m.end()) {
    // key 존재
}

// 존재여부 (0 또는 1 반환)
if (m.count("apple")) {
    // 존재
}

// 명시적 삽입, 이미 존재하면 삽입 안됨
m.insert({"apple", 5});

// 삭제
m.erase("apple");

// 원소 개수
int n = m.size();

// 전체 삭제
m.clear();
```

