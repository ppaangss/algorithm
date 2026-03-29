# 풀이

처음에 일단, 2차원 dp를 사용해서 풀었다.




```
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


/*
3 9
1
3
6

*/
```