
# sort의 3번째 인자

sort의 3번째 인자는 정렬 기준을 정할 수 있다.

보통 bool 함수를 만들어서 할 수 도있는데 람다함수를 써서 간편하게 할 수 있다.

**람다함수 사용하기**

```cpp

#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> v = {3, 1, 4, 1, 5, 9};

    // 람다 함수를 사용한 내림차순 정렬
    sort(v.begin(), v.end(), [](int a, int b) {
        return a > b; // 왼쪽 요소(a)가 오른쪽(b)보다 크면 true -> 큰 게 앞으로
    });

    for (int n : v) cout << n << " "; // 출력: 9 5 4 3 1 1
    return 0;
}

```

**bool 함수 사용하기**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// 1. 비교용 일반 함수 정의
bool compare(int a, int b) {
    return a > b; // 내림차순 정렬 기준
}

int main() {
    std::vector<int> v = {3, 1, 4, 1, 5};

    // 2. 함수의 이름을 세 번째 인자로 전달
    std::sort(v.begin(), v.end(), compare);

    for (int n : v) std::cout << n << " "; // 출력: 5 4 3 1 1
    return 0;
}
```

# 캡처

만약에 람다함수 방식을 쓰는 경우 함수 내에서 지역변수를 참고해야 할 수 있는 상황이 나올 수 있다. 그때 캡처에 값을 넣어 사용해야 한다.

`[변수]` 이렇게 사용하면 된다.

프로그래머스 테이블 해시 캡처를 사용함. 

예를들어, 2차원벡터가 있고 정렬을 할 때 기준을 세워야함. 근데 이제 람다 함수 외부 변수를 사용해야 한다. 

col-1을 기준으로 오름차순, 같으면 맨 앞 컬럼 기준 내림차순을 해야하는 상황에서는 이렇게 캡처를 이용해 구현한다.

```cpp
int solution(vector<vector<int>> data, int col) {
    int N = data.size();
    int M = data[0].size();
    
    sort(data.begin(),data.end(),[col](vector<int> a, vector<int> b){
        if(a[col-1] == b[col-1]){
            return a[0] > b[0];
        }
        return a[col-1] < b[col-1];
    });

```

