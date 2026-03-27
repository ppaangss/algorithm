
온도의 합들을 매번 계산하면 시간초과가 생긴다.

N개를 K번 해야하므로 최악의 경우 100000 x 100000 으로 시간초과가 난다.

내가 푼 방식은 슬라이딩 윈도우 방식이었다.
기준 하나를 잡아서 한 방향으로 이동하며 더하거나 뺴주는 방식이다.

또 다른 방식으로 알고리즘 종류로 누적 합을 이용하면 됐다.

## 풀이

- 슬라이딩 윈도우

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

// 컨테이너
#include <unordered_map>
#include <queue>

#define INF 87654321

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    int N; cin >> N;
    int K; cin >> K;

    vector<int> v;

    for (int i = 0; i < N; i++) {
        int tmp; cin >> tmp;
        v.push_back(tmp);
    }

    vector<int> res(N - K + 1, 0);

    for (int i = 0; i < K; i++) {
        res[0] += v[i];
    }

    for (int i = 1; i < res.size(); i++) {
        res[i] = res[i - 1] - v[i - 1] + v[i + K - 1];
    }

    int maxValue = *max_element(res.begin(), res.end());


    cout << maxValue << "\n";
}
```


