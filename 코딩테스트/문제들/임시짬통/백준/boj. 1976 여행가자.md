https://www.acmicpc.net/problem/1976
- 분리집합 (유니온 파인드)
# 회고

일단 단순히 N이 200 이하여서 플로이드 워셜로 풀었다. 근데 막상 풀어보니 최단 거리를 구하는게 아니라, 가능한 경로가 있는지 확인하는 문제여서 조금 달랐지만 그래도 풀긴했다.

이 문제의 올바른 풀이는 유니온 파인드이다. 어쨋든 시작부터 끝까지 같은 집합에 있으면 갈 수 있는 여행지인 것이다. 그래서 유니온파인드를 써서 여행지가 전부 같은 집합인지만 확인하면 끝난다.

# 풀이

### 플로이드 워셜 (성공)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

#define INF 9876543321

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; 
	int M;

	cin >> N;
	cin >> M;
	vector<vector<int>> maps(N + 1, vector<int>(N + 1, 0)) ;

	for (int i = 1; i <= N; i++) {
		for (int j = 1; j <= N; j++) {
			int tmp; cin >> tmp;
			if (i == j) {
				maps[i][j] = 1;
			}
			else {
				maps[i][j] = tmp;
			}
		}
	}

	vector<int> gt(M);

	for (int i = 0; i < M; i++) {
		cin >> gt[i];
	}

	for (int k = 1; k <= N; k++) {
		for (int i = 1; i <= N; i++) {
			for (int j = 1; j <= N; j++) {
				if (maps[i][j] == 0) { // 0이면 갈 방법이 없는데 k를 지나서 갈 수 있는지 확인
					if (maps[i][k] == 1 && maps[k][j] == 1) {
						// k를 지나서 가능하다.
						maps[i][j] = 1;
					}
				}
			}
		}
	}

	//for (int i = 1; i <= N; i++) {
	//	for (int j = 1; j <= N; j++) {
	//		cout << maps[i][j] << " ";
	//	}cout << "\n";
	//}cout << "\n";

	bool flag = true;
	for (int i = 1; i < gt.size(); i++) {
		if (maps[gt[i-1]][gt[i]] == 0) {
			flag = false;
			break;
		}
	}

	if (flag) {
		cout << "YES" << "\n";
	}
	else {
		cout << "NO" << "\n";
	}

	return 0;
}


/*

5
4
0 1 0 0 0
1 0 0 1 0 
0 0 0 0 1
0 1 0 0 0
0 0 1 0 0
1 4 2 5



*/
```

### 유니온 파인드

```cpp
#include <iostream>
#include <vector>
#include <algorithm>


using namespace std;

vector<int> parent;
vector<int> sz;

int FIND(int a) {
	if (parent[a] == a) return a;
	// 이게 경로 압축임 루트노드를 모든 노드에 저장
	else return parent[a] = FIND(parent[a]);
}

void UNION(int a, int b) {
	// 부모 찾기
	a = FIND(a);
	b = FIND(b);

	// 부모가 다른경우 합치기
	if (a != b) {
		if (sz[a] >= sz[b]) {
			parent[b] = a;
			sz[a] += sz[b];
		}
		else {
			parent[a] = b;
			sz[b] += sz[a];
		}
	}
}


int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	int N; 
	int M;

	cin >> N;
	cin >> M;

	parent.assign(N+1, 0);
	for (int i = 1; i <= N; i++) {
		parent[i] = i;
	}
	sz.assign(N + 1, 1);

	////디버깅
	//for (int i = 1; i <= N; i++) {
	//	cout << "parent-" << parent[i] << " ";
	//}cout << "\n";
	//for (int i = 1; i <= N; i++) {
	//	cout << "sz-" << sz[i] << " ";
	//}cout << "\n";

	//for (int i = 0; i < M; i++) {
	//	cout << "FIND-" << FIND(i + 1) << " ";
	//}cout << "\n";

	for (int i = 1; i <= N; i++) {
		for (int j = 1; j <= N; j++) {
			int tmp = 0; cin >> tmp;
			if (tmp == 1) {
				UNION(i, j);
			}
		}
	}

	//for (int i = 0; i < M; i++) {
	//	cout << "FIND-" << FIND(i + 1) << " ";
	//}cout << "\n";

	vector<int> gt;

	for (int i = 0; i < M; i++) {
		int tmp; cin >> tmp;
		gt.push_back(tmp);
	}

	int s = FIND(gt[0]);
	bool flag = true;
	for (int i = 1; i < M; i++) {
		if (s != FIND(gt[i])) {
			flag = false;
			break;
		}
	}

	if (flag) {
		cout << "YES" << "\n";
	}
	else {
		cout << "NO" << "\n";
	}


	return 0;
}

/*

5
4
0 1 0 0 0
1 0 0 1 0
0 0 0 0 1
0 1 0 0 0
0 0 1 0 0
1 4 2 1

*/
```