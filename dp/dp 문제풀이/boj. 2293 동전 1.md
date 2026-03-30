# 개요

### 문제 유형

- dp

### 회고


# 해결과정

처음에 일단, 2차원 dp를 사용해서 풀었다. 근데 메모리초과로 인해 틀렸다.

이 문제는 중복 가능, 조합에 해당하는 배낭문제이다.






# 풀이

### 2차원 DP를 이용한 풀이 (실패 - 메모리초과)

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N;
	int K;

	cin >> N;
	cin >> K;

	vector<int> coin(N);

	for (int i = 0; i < N; i++) {
		cin >> coin[i];
	}

	sort(coin.begin(), coin.end());

	vector<vector<int>> dp(K + 1, vector<int>(N, 0));
	
	for (int j = 0; j < N; j++) {
		dp[0][j] = 1;
	}

	for (int i = 1; i <= K; i++) {
		for (int j = 0; j < N; j++) {
			if (i - coin[j] >= 0) { // 더 이상 비교 못함.
				dp[i][j] += dp[i - coin[j]][j];
			}

			if (j != 0) {
				dp[i][j] += dp[i][j - 1];
			}
		}
	}

	//for (int i = 1; i <= K; i++) {
	//	cout << "dp" << i << "\n";
	//	for (int j = 0; j < N; j++) {
	//		cout << dp[i][j] << " ";
	//	}cout << "\n";
	//}

	//cout << "정답" << "\n";
	cout << dp[K][N - 1] << "\n";
	return 0;
}
```

### 코인 중심

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N, K;
    cin >> N >> K;

    vector<int> coin(N);
    for (int i = 0; i < N; i++) {
        cin >> coin[i];
    }

    // 2차원 dp[K+1][N] 대신 1차원 dp[K+1]만 사용
    // 메모리: 10,001 * 4바이트 = 약 40KB (제한 4MB에 한참 못 미침)
    vector<int> dp(K + 1, 0);
    
    // 초기값: 0원을 만드는 방법은 1가지
    dp[0] = 1;

    for (int j = 0; j < N; j++) { // 동전 하나를 선택 (아이템 중심)
        for (int i = coin[j]; i <= K; i++) { // 그 동전으로 만들 수 있는 금액들 업데이트
            // dp[i] (기존값: j-1번째 동전까지 쓴 경우)
            // dp[i - coin[j]] (현재 j번째 동전을 써서 i를 만드는 경우)
            dp[i] += dp[i - coin[j]];
        }
    }

    cout << dp[K] << "\n";

    return 0;
}
```

