
https://school.programmers.co.kr/learn/courses/30/lessons/42577

**분류**
- 해시
- 정렬

무난하게 해시를 사용해 해결 가능하다.
사전순이라는 것을 찾을 수 있으면 정렬로도 풀 수 있다.

# 풀이

## 해시 ( 내가 구현 ) --

- 모든 접두어를 해시셋에 넣으면 매우 큰 공간복잡도를 사용함. 따라서 불필요함.

```c++
#include <iostream>
#include <vector>
#include <unordered_set>
#include <string>

using namespace std;

bool solution(vector<string> phone_book) {
    bool answer = true;

    unordered_set<string> us;

    for (auto x : phone_book) {
        for (int i = 1; i < x.size(); i++) {
            us.insert(x.substr(0, i));
        }
    }

    for (auto x : phone_book) {
        if (us.find(x) != us.end()) {
            return false;
        }
    }

    return true;
}

```

## 해시 ( 개선 )

- 먼저 전체 번호를 set에 저장
- 이후 각 번호의 접두어만 검사
- 접두어를 set에 넣지 않음

```c++
bool solution(vector<string> phone_book) {
    unordered_set<string> us(phone_book.begin(), phone_book.end());

    for (const auto& x : phone_book) {
        string prefix;
        for (int i = 0; i < x.size() - 1; i++) {
            prefix += x[i];
            if (us.find(prefix) != us.end()) {
                return false;
            }
        }
    }
    return true;
}

```

## 정렬

- 정렬을 한 뒤 앞과 뒤를 비교함. 
	- 사전순 정렬로 인해 앞의 단어는 뒤의 접두어가 될 수 있기 때문이다.
- 시간복잡도 O(N log N)

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool solution(vector<string> phone_book) {
    sort(phone_book.begin(), phone_book.end());

    for (int i = 0; i < phone_book.size() - 1; i++) {
        if (phone_book[i + 1].find(phone_book[i]) == 0) {
            return false;
        }
    }
    return true;
}

```