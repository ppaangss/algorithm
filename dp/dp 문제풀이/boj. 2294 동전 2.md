# 개요
### 
### 유형

- dp

### 회고

이게 왜 앞에서부터 해결해도 되는지를 모르겠다.

일단 중복이 가능하고, 합이 K가 되도록 하는 최소 동전 개수를 구하는 문제는 일반적인 가치중심 반복문으로 해결가능하다.

근데 진짜 아이템 중심 반복문은 진짜 어떻게 쓰는건지 모르겠다.
# 해결과정

$dp[i]$ 에 대하여 인덱스 $i$ 는 가치, 값 $dp[i]$ 는 최소 동전 개수로 지정했다.

그렇게 해서 무난하게 가치 중심 반복문을 돌려서 코인마다 순회해주면 끝난다.

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

#define INF 987654321

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N, K;
    cin >> N >> K;

    vector<int> coin(N);
    for (int i = 0; i < N; i++) {
        cin >> coin[i];
    }

    vector<int> dp(K + 1, INF); // 동전을 사용한 최소 개수

    dp[0] = 0;

    for (int i = 0; i <= K; i++) {
        for (int j = 0; j < coin.size(); j++) {
            if (i - coin[j] > 0) {
                dp[i] = min(dp[i - coin[j]] + 1, dp[i]);
            }
            else if(i - coin[j] == 0){
                dp[i] = 1;
            }
        }

        //cout << "i-" << i << " dp[i]-" << dp[i] << "\n";
    }

    //for (int i = 1; i <= K; i++) {
    //    cout << dp[i] << " ";
    //}cout << "\n";
    if (dp[K] == INF) {
        cout << -1 << "\n";
    }
    else {
        cout << dp[K] << "\n";
    }
    return 0;
}
```