https://www.acmicpc.net/problem/16234
# 회고

시뮬레이션 + BFS 문제였다. 무난하게 해결 가능하지만, 약간의 코드를 수정하면 좋을 것 같다.

1. BFS 내부의 국경 개방 조건 오류 (가장 중요)

현재 코드의 BFS 로직 중 이 부분이 문제입니다:

```cpp
int dis = abs(v[cx][cy] - v[nx][ny]); // 현재 칸(cx, cy)과 다음 칸(nx, ny)의 차이
```

BFS는 이미 방문한 칸에서 다음 칸으로 뻗어 나가는 구조입니다. 따라서 `v[cx][cy]`와 `v[nx][ny]`의 차이를 구하면, 큐에 들어있는 '방금 방문한 칸'과 '옆 칸'을 비교하게 됩니다. 이는 논리적으로 맞으나, `visited[nx][ny]`를 업데이트하는 시점에 주의해야 합니다.

더 큰 문제는 BFS가 시작될 때 첫 번째 칸만 방문 처리를 하고 큐에 넣는 로직이 중복되거나 빠질 수 있다는 점입니다. (작성하신 코드에서는 다행히 `visited[x][y] = cnt1;`을 두 번 하며 시작하므로 시작점 문제는 없지만, 아래의 '연합 감지' 로직과 결합되어 무한 루프 위험이 있습니다.)

2. 종료 조건 (`cnt1 == 1 + N * N`)의 위험성

```cpp
if (cnt1 == 1 + N * N) break;
```

이 조건은 "모든 칸이 각각 독립된 연합일 때(즉, 이동이 하나도 없을 때)"를 의미합니다. 하지만 이론적으로는 맞으나, `cnt1`이 정확히 이 숫자가 되지 않는 예외 상황이 발생할 수 있습니다.

- 더 안전한 방법: `bool` 변수(예: `is_moved`)를 하나 두고, BFS 과정에서 `union_list.size() > 1`인 경우가 한 번이라도 발생하면 `true`로 바꿔주는 방식이 훨씬 확실합니다.

3. `unordered_map` 사용에 따른 오버헤드

매 루프마다 모든 칸을 순회하며 `unordered_map`에 접근하고, 다시 모든 칸을 순회하며 인구를 갱신합니다.

- 비효율: $N=50$일 때 $N^2 = 2500$입니다. 맵 접근 비용보다, BFS를 돌 때 해당 연합에 속한 좌표들을 `vector` 등에 바로 담아두고 BFS가 끝나자마자 즉시 인구를 평균치로 업데이트하는 것이 훨씬 빠릅니다.


대충 이런 문제가 있는데 솔직히 말하면 이거는 고치기에는 상당히 어려울 것으로 예상된다.

# 풀이

내가 푼 풀이

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <queue>
#include <cmath>
#include <unordered_map>

using namespace std;

vector<vector<int>> v;
vector<vector<int>> visited;

int dx[4] = { -1,0,1,0 };
int dy[4] = { 0,1,0,-1 };
int N;
int L;
int R;

void BFS(int x,int y,int cnt1) {

	visited[x][y] = cnt1;

	queue<pair<int,int>> qu;
	qu.push({ x,y });
	visited[x][y] = cnt1;

	while (!qu.empty()) {
		int cx = qu.front().first;
		int cy = qu.front().second;
		qu.pop();

		for (int i = 0; i < 4; i++) {

			int nx = cx + dx[i];
			int ny = cy + dy[i];

			if (nx >= 0 && nx < N && ny >= 0 && ny < N) {
				if (visited[nx][ny] == 0) { // 방문 안했어야해
					int dis = abs(v[cx][cy] - v[nx][ny]);

					if (dis >= L && dis <= R) { // 국경선이 열리는 지
						qu.push({ nx,ny });
						visited[nx][ny] = cnt1;
					}
				}
			}
		}
	}

}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> N;
	cin >> L;
	cin >> R;
	
	v.assign(N, vector<int>(N,0));
	visited.assign(N, vector<int>(N,0));

	for (int i = 0; i < N; i++) {
		for (int j = 0; j < N; j++) {
			cin >> v[i][j];
		}
	}

	int cnt2 = 0; // 얼마나 국경선이 열려있었나.
	while (1) {

		for (auto& row : visited) {
			row.assign(N, 0);
		}

		int cnt1 = 1; // 연합국 집합 식별자
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				if (visited[i][j] == 0) {
					BFS(i, j, cnt1);
					cnt1++;
				}
			}
		}

		//cout << cnt1 << "\n";

		if (cnt1 == 1 + N * N) { // 종료, 더이상 국경선이 열릴 수 없음.
			//cout << "종료" << "\n";
			break;
		}

		unordered_map<int, pair<int, int>> um;

		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				int index1 = visited[i][j];
				um[index1].first++;
				um[index1].second += v[i][j];
			}
		}

		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				int index1 = visited[i][j];
				v[i][j] = um[index1].second / um[index1].first;
			}
		}

		//for (auto x : um) {
		//	cout << x.first << x.second.first << x.second.second << "\n";
		//}

		//// 디버깅
		//for (int i = 0; i < N; i++) {
		//	for (int j = 0; j < N; j++) {
		//		cout << v[i][j] << " ";
		//	}cout << "\n";
		//}

		cnt2++;
	}

	cout << cnt2 << "\n";

	return 0;
}


```

재미나이

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>
#include <cstring>

using namespace std;

int N, L, R;
int map[51][51];
bool visited[51][51];
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

// 한 번의 BFS는 하나의 '연합'을 찾음
bool bfs(int r, int c) {
    vector<pair<int, int>> union_list; // 연합에 속한 국가 좌표
    queue<pair<int, int>> q;
    
    q.push({r, c});
    union_list.push_back({r, c});
    visited[r][c] = true;
    
    int sum = map[r][c];
    
    while (!q.empty()) {
        int x = q.front().first;
        int y = q.front().second;
        q.pop();
        
        for (int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            
            if (nx >= 0 && nx < N && ny >= 0 && ny < N && !visited[nx][ny]) {
                int diff = abs(map[x][y] - map[nx][ny]);
                if (diff >= L && diff <= R) {
                    visited[nx][ny] = true;
                    q.push({nx, ny});
                    union_list.push_back({nx, ny});
                    sum += map[nx][ny];
                }
            }
        }
    }
    
    // 연합국이 자기 자신 외에 더 있다면 인구 이동 발생
    if (union_list.size() > 1) {
        int avg = sum / union_list.size();
        for (auto p : union_list) {
            map[p.first][p.second] = avg;
        }
        return true;
    }
    return false;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    
    cin >> N >> L >> R;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cin >> map[i][j];
        }
    }
    
    int days = 0;
    while (true) {
        bool is_moved = false;
        memset(visited, false, sizeof(visited));
        
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (!visited[i][j]) {
                    if (bfs(i, j)) is_moved = true;
                }
            }
        }
        
        if (!is_moved) break; // 더 이상 이동이 없으면 종료
        days++;
    }
    
    cout << days << "\n";
    
    return 0;
}
```