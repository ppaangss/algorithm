# 개요

### [boj. 14500 테트로미노](https://www.acmicpc.net/problem/14500)

### 유형

- DFS
- 시뮬레이션
- 브루트포스
### 회고

DFS 문제의 응용 버전에 관한 문제였다. 이런 문제도 나올 수 있다는 거 알아두자.
# 해결과정

처음에는 단순히 4개를 구성하는 DFS 문제인 줄 알았다.

근데 문제가 'ㅗ' 모양은 DFS로 만들 수 없다는 것이었다.

그래서 여기서 2가지를 생각했다. 

1. 3개짜리를 만들고 3개짜리에서 모든 방향을 고려하기
2. 'ㅗ' 모양만 따로 처리하기

## 3개짜리를 만들고 3개짜리에서 모든 방향을 고려하기

일단 1번으로 풀어봤는데 당연히 시간초과가 떴다.

일단은 방식은 현재까지 방문한 모든 칸에서 사방으로 뻗어나가는 방식이다. 따라서 4 x (2 x 4) X ( 3 X 4 ) = 384번의 재귀 호출이 발생할 가능성이 있다.

거기에 set을 사용한다면 매우큰 시간복잡도가 생긴다.

최악의 경우 500 x 500 x 384 x & 하면은 시간초과가 무조건 생긴다.

## 'ㅗ' 모양만 따로 처리하기

그래서 답은 2번이었다. 왜냐하면 'ㅗ' 모양 자체를 구하는 시간복잡도는 단순히 $O(4)$ 밖에 걸리지 않는다. 한 점으로부터 4번 계산해주면 되기 때문이다. 그래서 간단하게 DFS + 'ㅗ' 모양 따로 구하기로 풀 수 있었다.

DFS의 경우에 4 x 3 x 3 = 36번의 연산이 발생하며 최악의 경우 500 x 500 x 4 로 안정적으로 구할 수 있다 따라서 시간복잡도는 $O( N \times M)$ 이다.

이렇듯 DFS의 구조가 깨지는 모양을 구해야 할때는 모양이 정해져 있다면 노가다성을 이용해 구해주는 것이 바람직한 것 같다.

모양이 가변적이면 상태(State)를 포함한 DP나 비트마스킹을 활용한 BFS 를 생각해 볼 수 있다.

# 풀이

### 1번 풀이 (실패 - 시간초과)

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <queue>
#include <set>
using namespace std;

int N;
int M;
int max1 = 0;

int dx[4] = { -1,0,1,0 };
int dy[4] = { 0,1,0,-1 };

vector<vector<int>> v;
//vector<vector<bool>> visited;
set<pair<int, int>> visited;

void DFS(int sum, int depth) {

	if (depth == 3) {

		//for (const auto& vst : visited) {
		//	cout << "x-" << vst.first << " y-" << vst.second << "\n";
		//}
		//cout << "sum-" << sum << "\n";
		//cout << "depth-" << depth << "\n";

		if (max1 < sum) {
			max1 = sum;
		}

		return;
	}

	for (const auto& vst : visited) {
		for (int i = 0; i < 4; i++) {
			int nx = vst.first + dx[i];
			int ny = vst.second + dy[i];

			//cout << "nx-" << nx << " ny-" << ny << "\n";

			if (nx >= 0 && nx < N && ny >= 0 && ny < M) {
				if (visited.find({ nx,ny }) == visited.end()) {
					//cout << "nx-" << nx << " ny-" << ny << "\n";
					visited.insert({ nx,ny });
					DFS(sum + v[nx][ny], depth + 1);
					visited.erase({ nx,ny });
				}
			}
		}
	}


}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> N;
	cin >> M;

	//visited.assign(N,vector<bool>(M,false));

	for (int i = 0; i < N; i++) {
		vector<int> tmp;
		for (int j = 0; j < M; j++) {
			int tmp1; cin >> tmp1;
			tmp.push_back(tmp1);
		}
		v.push_back(tmp);
	}

	//// 디버깅
	//for (int i = 0; i < N; i++) {
	//	for (int j = 0; j < M; j++) {
	//		cout << visited[i][j] << " ";
	//	}cout << "\n";
	//}


	//visited[0][0] = true;
	//DFS(v[0][0], 0, 0, 0);
	//visited[0][0] = false;


	for (int i = 0; i < N; i++) {
		for (int j = 0; j < M; j++) {
			visited.insert({ i,j });
			DFS(v[i][j], 0);
			visited.erase({ i,j });
		}
	}

	cout << max1 << "\n";


	return 0;
}
```

### 2번 풀이 (성공)

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <queue>
using namespace std;

int N;
int M;
int max1 = 0;

int dx[4] = { -1,0,1,0 };
int dy[4] = { 0,1,0,-1 };

vector<vector<int>> v;
vector<vector<bool>> visited;

void DFS(int sum, int depth,int x,int y) {

	if (depth == 3) {

		//cout << "sum-" << sum << "\n";
		//cout << "depth-" << depth << "\n";
		//cout << "x-" << x << "\n";
		//cout << "y-" << y << "\n\n";

		if (max1 < sum) {
			max1 = sum;
		}

		return;
	}

	for (int i = 0; i < 4; i++) {
		int nx = x + dx[i];
		int ny = y + dy[i];

		//cout << "nx-" << nx << " ny-" << ny << "\n";

		if (nx >= 0 && nx < N && ny >= 0 && ny < M) {
			if (visited[nx][ny] == false) {
				//cout << "nx-" << nx << " ny-" << ny << "\n";
				visited[nx][ny] = true;
				DFS(sum + v[nx][ny], depth + 1, nx, ny);
				visited[nx][ny] = false;
			}
		}
	}
}

void FUNC(int x, int y) {

	int sum = v[x][y];

	int dsum[4][3] = {
		{0,1,2},
		{0,1,3},
		{0,2,3},
		{1,2,3}
	};

	for (int i = 0; i < 4; i++) {

		sum = v[x][y];
		int cnt = 0; // 3개면 통과
		for (int j = 0; j < 3; j++) {
			int nx = x + dx[dsum[i][j]];
			int ny = y + dy[dsum[i][j]];

			if (nx >= 0 && nx < N && ny >= 0 && ny < M) {
				sum += v[nx][ny];
				cnt++;
			}
		}

		if (cnt == 3) {
			//cout << sum << "\n";
			if (max1 < sum) {
				max1 = sum;
			}
		}

	}
}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> N;
	cin >> M;

	visited.assign(N,vector<bool>(M,false));

	for (int i = 0; i < N; i++) {
		vector<int> tmp;
		for (int j = 0; j < M; j++) {
			int tmp1; cin >> tmp1;
			tmp.push_back(tmp1);
		}
		v.push_back(tmp);
	}

	//// 디버깅
	//for (int i = 0; i < N; i++) {
	//	for (int j = 0; j < M; j++) {
	//		cout << visited[i][j] << " ";
	//	}cout << "\n";
	//}

	//// 디버깅
	//visited[0][0] = true;
	//DFS(v[0][0], 0, 0, 0);
	//visited[0][0] = false;

	//FUNC(2, 2);

	for (int i = 0; i < N; i++) {
		for (int j = 0; j < M; j++) {
			visited[i][j] = true;
			DFS(v[i][j], 0, i, j);
			visited[i][j] = false;
			FUNC(i, j);
		}
	}

	cout << max1 << "\n";

	return 0;
}
```