# 개요
### [boj. 12865 평범한 배낭](https://www.acmicpc.net/problem/12865)
### 유형

- dp
### 회고

상태에 따른 차원 축소를 어떤 상황에서 할 수 있는지에 대해 잘 알아놔야 겠다.

[배낭문제 1차원 vs 2차원](배낭문제%201차원%20vs%202차원.md)
# 해결과정

N개의 물건을 중복없이 무게 W와 가치 V에 대해 최대 K만큼의 무게에 대해서 최대 가치를 구하는 문제이다.

즉, 상태가 2가지인 2차원 dp를 사용하여 풀 수 있다.

하지만 문제가 최대 가치만 구하면 되므로 인덱스-값을 무게-가치로 dp를 설정해서 1차원 dp로축소해서 풀 수 도있다.


# 풀이

### 1차원 dp (성공)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N;
	int K;

	cin >> N >> K;

	vector<pair<int,int>> vt;

	for (int i = 0; i < N; i++) {
		int tmp; cin >> tmp;
		int tmp2; cin >> tmp2;

		vt.push_back({ tmp,tmp2 });
	}

	vector<int> dp(K+1,0);

	for (int i = 0; i < N; i++) {
		int wei = vt[i].first;
		int val = vt[i].second;

		for (int j = dp.size()-1; j >= wei; j--) {
			if (dp[j - wei] + val > dp[j]) {
				dp[j] = dp[j - wei] + val;
			}
		}
	}

	cout << dp[K] << "\n";

	return 0;
}
```