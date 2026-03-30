# 개요

### [boj. 2156 포도주 시식](https://www.acmicpc.net/problem/2156)

### 유형

- dp

### 회고

dp를 어떻게 정의하냐가 굉장히 중요하고, 그 다음으로는 점화식을 굉장히 꼼꼼하게 확인해야한다.

이 문제의 경우 계단오르기랑 똑같은 줄 알았는데 큰 예외가 하나 있었다.

$i$ 번째 포도주에 대한 $dp[i]$ 의 경우 $i$ 번째 포도주를 꼭 마실 필요가 없다는 것이다.

따라서 이 조건으로 인해 규칙이 더 많아졌다. 그걸 잘 캐치하고 문제를 풀어야한다.
# 해결과정

결국 3가지 경우의 수가 있다.

1. - 이번 포도주를 안 마시는 경우
	- 직전까지의 최댓값 그대로 가져옴 $\rightarrow dp[i-1]$
    
2. 이번 포도주를 마시고, 직전 건 안 마신 경우
	- (전전까지의 최댓값) + (현재 포도주) $\rightarrow dp[i-2] + wine[i]$
    
3. 이번 포도주를 마시고, 직전 것도 마신 경우
	- (연속 3잔 금지이므로 전전전까지의 최댓값) + (직전 포도주) + (현재 포도주) $\rightarrow dp[i-3] + wine[i-1] + wine[i]$

이 3가지를 처리하면 끝난다.

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int n;
    cin >> n;

    vector<int> wine(n + 1, 0);
    vector<int> dp(n + 1, 0);

    for (int i = 1; i <= n; i++) {
        cin >> wine[i];
    }

    // 초기값 설정
    dp[1] = wine[1];
    if (n >= 2) {
        dp[2] = wine[1] + wine[2];
    }

    // DP 진행
    for (int i = 3; i <= n; i++) {
        // 1. i번째를 안 마심 (dp[i-1])
        // 2. i번째 마심, i-1번째 안 마심 (dp[i-2] + wine[i])
        // 3. i번째 마심, i-1번째 마심 (dp[i-3] + wine[i-1] + wine[i])
        dp[i] = max({dp[i - 1], dp[i - 2] + wine[i], dp[i - 3] + wine[i - 1] + wine[i]});
    }

    cout << dp[n] << "\n";

    return 0;
}
```



