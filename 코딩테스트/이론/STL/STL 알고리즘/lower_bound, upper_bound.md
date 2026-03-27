
# 개요

- 이진탐색을 기반으로 특정 값의 경계를 찾는 함수이다.
- 데이터가 오름차순으로 정렬되어 있어야 한다.
- algorithm 라이브러리
- 

# lower_bound

찾으려는 기준값 $X$보다 **크거나 같은 첫 번째 원소**의 위치(반복자, Iterator)를 반환한다.

특정 값이 시작되는 위치를 찾을 때 유용하다.


# upper_bound

찾으려는 기준값 $X$를 **초과하는 첫 번째 원소**의 위치(반복자, Iterator)를 반환한다.

특정 값이 끝나는 지점 바로 다음 위치를 찾을 때 유용합니다.

| **구분**      | **lower_bound**               | **upper_bound**               |
| ----------- | ----------------------------- | ----------------------------- |
| **조건**      | $Value \ge X$                 | $Value > X$                   |
| **반환값**     | 기준값 이상의 첫 번째 위치               | 기준값 초과 첫 번째 위치                |
| **값이 없을 때** | $X$보다 큰 첫 번째 원소 (없으면 `end()`) | $X$보다 큰 첫 번째 원소 (없으면 `end()`) |

# 예시 코드

```cpp

#include <iostream>
#include <vector>
#include <algorithm> // lower_bound, upper_bound 포함

using namespace std;

int main() {
    vector<int> v = {10, 20, 30, 30, 30, 40, 50};

    // 30이 처음 나타나는 위치
    auto low = lower_bound(v.begin(), v.end(), 30);
    // 30을 초과하는 첫 번째 위치
    auto up = upper_bound(v.begin(), v.end(), 30);

    cout << "lower_bound(30) index: " << distance(v.begin(), low) << endl; // 인덱스: 2
    cout << "upper_bound(30) index: " << distance(v.begin(), up) << endl;  // 인덱스: 5
    cout << "30의 개수: " << up - low << endl; // 3개

    return 0;
}

```

