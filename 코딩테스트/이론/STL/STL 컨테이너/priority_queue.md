# 개요

- 내부의 원소 중 우선순위가 가장 높은 데이터가 먼저 나가는 자료구조이다.
- 일반적인 큐(FIFO)와 달리, 들어온 순서에 상관없이 특정 기준에 따라 정렬된 상태로 추출된다.
## 활용

- 다익스트라(Dijkstra) 알고리즘: 최단 경로를 찾을 때 거리가 가장 짧은 노드를 먼저 탐색하기 위해 사용.
# 설명

우선순위 큐는 데이터를 삽입할 때마다 내부적으로 정렬을 수행하여, 항상 '가장 우선순위가 높은 값'이 맨 위에 오도록 유지하는 자료구조입니다. 따라서 이미 존재하는 값을 수정하는 것은 불가능하며, 새로운 값을 삽입하거나 맨 위의 값을 삭제하는 과정에서 무조건 재정렬이 일어납니다.
## 특징

- 힙(Heap) 구조: 내부적으로 완전 이진 트리인 힙 구조를 사용하여 구현되어 있다.
- 자동 정렬: 데이터가 삽입(`push`)되거나 삭제(`pop`)될 때마다 자동으로 위치를 찾아 정렬하므로 삽입/삭제 성능이 우수하다.
- 제한적 접근: 인덱스를 통한 중간 값 접근이나 수정이 불가능하며, 오직 맨 위의 값(`top`)만 확인할 수 있다.
- 사용자 정의 정렬: 기본값은 내림차순(최대 힙)이지만, 구조체나 비교 함수를 통해 원하는 정렬 기준을 직접 설정할 수 있다.
- 내부적으로 힙(Heap) 구조를 사용하여 구현되어 있다.
	- 삽입, 삭제 성능이 좋다.
- 데이터가 들어올 때와 나갈 때 자동으로 위치를 찾아 정렬한다.
- 인덱스로 접근할 수 없으며, 중간에 있는 값을 직접 수정할 수 없다.
	- 오직 맨 위의 값 (top) 만 확인할 수 있다.
- 사용자 정의 정렬을 통해 특정 조건을 기준으로 정렬할 수 있다.
	- 기본값은 내림차순이다.  
	- "우선순위가 낮을 수록 뒤로 보낸다." 를 기준으로 생각하면 이해가 편하다.
	- 사용자 정의 정렬을 할 때 세 번째 인자로 들어가는 것은 함수 호출이 아닌 타입이므로 구조체가 필요하다.

## 자주 쓰는 멤버 함수
| 함수            | 설명                        | 시간 복잡도      |
| ------------- | ------------------------- | ----------- |
| `push(Value)` | 원소를 추가하고 재정렬함             | $O(\log n)$ |
| `pop()`       | 맨 위(우선순위가 가장 높은) 원소를 제거   | $O(\log n)$ |
| `top()`       | 맨 위 원소를 반환 (제거 X)         | $O(1)$      |
| `empty()`     | 비어있으면 `true`, 아니면 `false` | $O(1)$      |
| `size()`      | 현재 원소의 개수 반환              | $O(1)$      |

# 예시
**일반 int에 대한 사용자 정의 함수**
```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

// 컨테이너
#include <queue>

struct Compare {
    // () 연산자를 오버로딩하여 함수처럼 동작하게 만듭니다.
    bool operator()(int a, int b) {
        // true를 반환하면 a가 b보다 우선순위가 낮다고 판단하여 뒤로 보냅니다.
        // 즉, 'a > b' 이면 오름차순(최소 힙)이 됩니다.
        return a > b;
    }
};

int main() {
    // 세 번째 인자에 비교용 구조체 타입을 넣어줍니다.
    priority_queue<int, vector<int>, Compare> pq;

    pq.push(10);
    pq.push(20);
    pq.push(5);

    while (!pq.empty()) {
        cout << pq.top() << " "; // 출력: 5 10 20
        pq.pop();
    }
}
```

**구조체를 활용한 비교 방식**
```cpp 
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

// 컨테이너
#include <queue>

struct Student {
    int id;
    int score;

    // 점수가 높은 순으로, 점수가 같다면 ID가 낮은 순으로 정렬하고 싶을 때
    bool operator<(const Student& s) const {
        if (score == s.score) return id > s.id;
        return score < s.score;
    }
};

int main() {
    // 세 번째 인자에 비교용 구조체 타입을 넣어줍니다.
    priority_queue<Student> pq;

    pq.push({ 1,10 });
    pq.push({ 2,20 });
    pq.push({ 3,5 });

    while (!pq.empty()) {
        cout << pq.top().id << " " << pq.top().score << "\n"; // 출력: 5 10 20
        pq.pop();
    }
}
```