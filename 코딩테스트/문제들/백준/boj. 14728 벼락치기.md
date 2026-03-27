

## dp

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int dp[10001];

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; cin >> N;
	int T; cin >> T;

	vector<pair<int, int>> v;

	for(int i =0;i<N;i++){
		int tmp1; cin >> tmp1;
		int tmp2; cin >> tmp2;

		v.push_back({ tmp1,tmp2 });
	}

	for (int i = 0; i <= T; i++) {

		cout << "i-" << i << "\n";
		for (int x = 0; x < v.size(); x++) {
			int cx1 = v[x].first; // 시간
			int cx2 = v[x].second; // 점수

			if (i - cx1 < 0) { // 공부할 수 없는 단원
				continue;
			}

			dp[i] = max(dp[i], dp[i - cx1] + cx2);
		}

		cout << "dp-" << dp[i] << "\n";
	}

	return 0;
}

/*





*/
```

## 백트래킹 - 실패

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int dp[10001];
vector<pair<int, int>> v;
//vector<bool> visited;

int time1 = 0; // 현재 쓴 시간
int score1 = 0; // 현재 점수
int max1 = 0; // 최대 점수
int N;
int T;
void DFS(int n) {

	if (n == v.size()) { // 더 이상 볼게 없다.
		if (max1 < score1) {
			max1 = score1;
		}
		return;
	}


	if (time1 + v[n].first <= T) { // 현재 남은 시간으로 가능해야함.
		//visited[i] = true;

		time1 += v[n].first;
		score1 += v[n].second;

		//cout << "먹" << "\n";
		//cout << "n-" << n << "\n";
		//cout << "time1-" << time1 << "\n";
		//cout << "score1-" << score1 << "\n\n";

		DFS(n + 1);

		//visited[i] = false;
		time1 -= v[n].first;
		score1 -= v[n].second;
	}
	else {
		//cout << "못먹" << "\n";
		//cout << "n-" << n << "\n";
		//cout << "time1-" << time1 << "\n";
		//cout << "score1-" << score1 << "\n\n";
	}

	//cout << "안먹" << "\n";
	//cout << "n-" << n << "\n";
	//cout << "time1-" << time1 << "\n";
	//cout << "score1-" << score1 << "\n\n";

	DFS(n+1); // 방문 안할거야.
	
	
	return;
}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> N;
	cin >> T;

	for(int i =0;i<N;i++){
		int tmp1; cin >> tmp1;
		int tmp2; cin >> tmp2;
		//visited.push_back(false);
		v.push_back({ tmp1,tmp2 });
	}

	DFS(0);

	cout << max1 << "\n";

	return 0;
}

/*





*/
```