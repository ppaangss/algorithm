
https://school.programmers.co.kr/learn/courses/30/lessons/86971

**분류**
- 완전탐색
- dfs

# 풀이
## dfs

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;

int bfs(vector<vector<int>> sjt, int n1,int n2) {
	
	vector<bool> visited(sjt.size()+1,false);

	queue<int> q;

	int cnt = 0;

	visited[n1] = true;
	cnt++;
	for (int i = 0; i < sjt[n1].size(); i++) {
		if (sjt[n1][i] == n2) {
			continue;
		}
		q.push(sjt[n1][i]);
	}

	while (!q.empty()) {
		int curN = q.front();
		q.pop();
		visited[curN] = true;
		cnt++;

		for (int i = 0; i < sjt[curN].size(); i++) {
			int tmpN = sjt[curN][i];

			if (visited[tmpN] == false) {
				q.push(sjt[curN][i]);	
			}
		}
	}

	return cnt;
}
int solution(int n, vector<vector<int>> wires) {

	int minN = 987654321;
	vector<vector<int>> sjt(n+1);

	// 송전탑 연결 트리 구현
	for (int i = 0; i < wires.size(); i++) {
		int f = wires[i].at(0);
		int e = wires[i].at(1);

		sjt[f].push_back(e);
		sjt[e].push_back(f);
	}

	// 연결된 거 하나 씩 끊고 bfs 돌리기
	for (int i = 0; i < wires.size(); i++) {
		int n1 = wires[i].at(0);
		int n2 = wires[i].at(1);

		int dis1 = bfs(sjt, n1 , n2 );

		// dfs를 하나 더 할 필요가 없음 
		// 전체 개수에서 빼주면 됨
		// int dis2 = bfs(sjt, n2 , n1 );
		int dis2 = n - dis1;

		if (dis1 > dis2) {
			//cout << dis1 - dis2 << "\n";
			minN = min(minN, dis1 - dis2);	
		}

		else {
			//cout << dis2 - dis1 << "\n";
			minN = min(minN, dis2 - dis1);
		}

	}

	return minN;
}

int main() {

//	vector<vector<int>> v = { {1,3} , {2,3},{3,4},{4,5},{4,6},{4,7},{7,8},{7,9} };
	vector<vector<int>> v = { {1,2} , {2,7},{3,7},{3,4},{4,5},{6,7} };

	cout << solution(9, v);

}


// n개 와이어 n-1
// 한번 자르면 무조건 2개로 분리됨
```


