컨테이너에서 특정 조건을 만족하는 요소의 개수를 세는 표준 알고리즘

``` cpp
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5, 6};

    int cnt = std::count_if(v.begin(), v.end(),
                            [](int x) { return x % 2 == 0; });

    std::cout << cnt; // 3 (2, 4, 6)
}
```