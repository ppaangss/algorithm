
# 회고

무난한 DFS 문제였다. 그러나 시간초과는 무난하지 않았다.

결국에는 백트래킹이 핵심이었고, 백트래킹을 할 때 기준을 잘 설정하고 빠른 자료구조를 써야한다는 것을 알았다.

백트래킹을 위한 visited 배열 구성 방식은 다음과 같다.

이제 데이터 범위가 고정적인지 아닌지부터 확인을 해야한다.

데이터 범위가 고정이면 무조건 배열을 쓰는게 좋다. 그래서 결국 베열을 써서 빠른 인덱스 접근과 삽입, 삭제를 사요해야한다.

너무 크면 이제 그때 다른 자료구조를 쓰면 좋을 것 같다.

그리고 이제 방문 상태가 비트 단위일 경우 비트마스킹을 써도 시간복잡도를 줄일 수 있다.

그리고 그 외에도 줄일 수 있는 시간복잡도 요소가 있었다. max 값 처리를 배열에 넣어 찾는게 아니라 그냥 max 변수 하나 써서 계속해서 변경해주는 식으로 해주는 것이 더 좋았다.


# 풀이

실패 (시간초과)

```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

//
#include <queue>
#include <unordered_set>

using namespace std;

#define INF 0

int dx[4] = { -1,0,1,0 };
int dy[4] = { 0, 1,0 ,-1 };

int R;
int C;

vector<vector<char>> map; // 맵
vector<int> cnt[20][20]; // 각 칸마다 최대 칸
unordered_set<char> st; // 지나왔던 알파벳

void DFS(int x, int y, int cnt1) {

	for (int i = 0; i < 4; i++) {
		int nx = x + dx[i];
		int ny = y + dy[i];

		if (nx >= 0 && nx < R && ny >= 0 && ny < C) { // 인덱스 내 범위
			char ch = map[nx][ny];
			if (st.find(ch) == st.end()) { // 한 번도 못본 알파벳
				st.insert(ch);
				cnt[nx][ny].push_back(cnt1+1); // 자리의 최단 거리
				DFS(nx, ny, cnt1+1);
				st.erase(ch);
			}
		}
	}
}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> R;
	cin >> C;

	for (int i = 0; i < R; i++) {
		string tmp; cin >> tmp;
		vector<char> tmp1;
		for (int j = 0; j < tmp.size(); j++) {
			tmp1.push_back(tmp[j]);
		}
		map.push_back(tmp1);
	}

	//for (int i = 0; i < R; i++) {
	//	for (int j = 0; j < C; j++) {
	//		cout << map[i][j] << " ";
	//	}cout << "\n";
	//}cout << "\n";

	st.insert(map[0][0]);
	cnt[0][0].push_back(1);
	DFS(0, 0, 1);

	int max1 = 1;

	//for (int i = 0; i < R; i++) {
	//	for (int j = 0; j < C; j++) {
	//		cout << "i-" << i << "_j-" << j << "\n";
	//		for (auto x : cnt[i][j]) {
	//			cout << x << " ";
	//		}cout << "\n";
	//	}cout << "\n";
	//}cout << "\n";

	for (int i = 0; i < R; i++) {
		for (int j = 0; j < C; j++) {
			for (auto x : cnt[i][j]) {
				if (max1 < x) {
					max1 = x;
				}
			}
		}
	}

	cout << max1 << "\n";


	return 0;
}

/*

5 5
ABCDE
FGHIJ
KLMNO
PQRST
UVWXX


5 5
ABCDE
FGHIJ
KLAAA
PQAST
UVAXX




*/

```


```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

//
#include <queue>
#include <unordered_set>

using namespace std;

#define INF 0

int dx[4] = { -1,0,1,0 };
int dy[4] = { 0, 1,0 ,-1 };

int R;
int C;

vector<vector<char>> map; // 맵
vector<int> cnt[20][20]; // 각 칸마다 최대 칸
unordered_set<char> st; // 지나왔던 알파벳
bool visited[26];
//int max1 = 0;

void DFS(int x, int y, int depth) {

	for (int i = 0; i < 4; i++) {
		int nx = x + dx[i];
		int ny = y + dy[i];

		if (nx >= 0 && nx < R && ny >= 0 && ny < C) { // 인덱스 내 범위
			int alphaIdx = map[nx][ny] - 'A';
			if (!visited[alphaIdx]) {
				//cout << "nx-" << nx << " ny-" << ny << "\n";
				visited[alphaIdx] = true;
				//cout << depth + 1 << "dd \n";
				cnt[nx][ny].push_back(depth + 1);
				DFS(nx, ny, depth + 1);
				visited[alphaIdx] = false; // 백트래킹
			}
		}
	}
}

int main() {

	ios_base::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> R;
	cin >> C;

	for (int i = 0; i < R; i++) {
		string tmp; cin >> tmp;
		vector<char> tmp1;
		for (int j = 0; j < tmp.size(); j++) {
			tmp1.push_back(tmp[j]);
		}
		map.push_back(tmp1);
	}

	//for (int i = 0; i < R; i++) {
	//	for (int j = 0; j < C; j++) {
	//		cout << map[i][j] << " ";
	//	}cout << "\n";
	//}cout << "\n";

	cnt[0][0].push_back(1);
	int tmp = map[0][0] - 'A';
	visited[tmp] = true;
	DFS(0, 0, 1);

	int max1 = 1;

	//for (int i = 0; i < R; i++) {
	//	for (int j = 0; j < C; j++) {
	//		cout << "i-" << i << "_j-" << j << "\n";
	//		for (auto x : cnt[i][j]) {
	//			cout << x << " ";
	//		}cout << "\n";
	//	}cout << "\n";
	//}cout << "\n";

	for (int i = 0; i < R; i++) {
		for (int j = 0; j < C; j++) {
			for (auto x : cnt[i][j]) {
				if (max1 < x) {
					max1 = x;
				}
			}
		}
	}

	cout << max1 << "\n";


	return 0;
}

/*

5 5
ABCDE
FGHIJ
KLMNO
PQRST
UVWXX


5 5
ABCDE
FGHIJ
KLAAA
PQAST
UVAXX




*/
```