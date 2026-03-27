
https://www.acmicpc.net/problem/2579

dp 중 좀 어려운 문제
점화식 생각하기가 까다로움

연속된 1칸 계단은 불가능하기때문에
1칸 계단에서 올라온 값의 경우에는 반드시 그 전에 2칸 계단에서 올라왔어야 한다.
그 외에 경우는 상관없음

## 풀이

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

// 컨테이너


#define INF 87654321

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N; cin >> N;

    vector<int> v;
    v.push_back(0);

    for (int i = 0; i < N; i++) {
        int tmp; cin >> tmp;
        v.push_back(tmp);
    }

    vector<int> dp(N + 1,0);

    dp[0] = 0;
    dp[1] = v[1];
    
    if (N > 1) {
        dp[2] = dp[1] + v[2];
    }

    for (int i = 3; i <= N; i++) {
        dp[i] = max(dp[i - 3] + v[i - 1], dp[i - 2]) + v[i];
    }

    cout << dp[N] << "\n";
}


/*

dp[3] = dp[0] + v[2]
or
dp[3] = dp[1]
*/
```