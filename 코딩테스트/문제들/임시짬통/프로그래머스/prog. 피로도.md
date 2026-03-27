
https://school.programmers.co.kr/learn/courses/30/lessons/87946

**분류**
- 완전탐색
- 순열 
- dfs

시간복잡도
- 던전 최대 8개 -> 8! = 40320 
- 완전탐색 가능

순열으로도 가능
dfs로 풀기

# 풀이

## 순열

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int solution(int k, vector<vector<int>> dungeons) {


	int answer = -1;

	vector<int> v;

	for (int i = 0; i < dungeons.size(); i++) {
		v.push_back(i);
	}
	// 최소소모피로도 내림차순 정렬

	// 각 던전을 하냐 안하냐를 모두 구함
	// 재귀?
	// DFS?
	int curNum = 0;
	int maxNum = 0;
	int curK = k;

	do {
		curNum = 0;
		curK = k;
		for (int i = 0; i < v.size(); i++) {
			if (dungeons[v[i]][0] <= curK) {
				curNum++;
				curK -= dungeons[v[i]][1];
			}
			else {
				break;
			}
		}

		if (maxNum < curNum) {
			maxNum = curNum;
		}

	} while (next_permutation(v.begin(), v.end()));


	return maxNum;
}

int main() {
	
	vector<vector<int>> v = { { 30,20 },{ 50,20 },{ 50,30 } ,{ 30,20 },{ 20,10 },{ 80,40 } };
	cout << solution(100, v);
}

// 약수 찾아
// 브라운 개수 구해 맞으면 ㄱ
// [[30, 20], [50, 20], [50, 30], [30, 20], [20, 10], [80, 40]]
```

## dfs + 백트래킹 (모범답안)

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int answer = 0;
vector<bool> visited;

void dfs(int fatigue, const vector<vector<int>>& dungeons, int count) {
    answer = max(answer, count);

    for (int i = 0; i < dungeons.size(); i++) {
        if (visited[i]) continue;

        int need = dungeons[i][0];
        int cost = dungeons[i][1];

        if (fatigue >= need) {
            visited[i] = true;
            dfs(fatigue - cost, dungeons, count + 1);
            visited[i] = false; // 백트래킹
        }
    }
}

int solution(int k, vector<vector<int>> dungeons) {
    visited.assign(dungeons.size(), false);
    dfs(k, dungeons, 0);
    return answer;
}

int main() {
	
	vector<vector<int>> v = { { 80,20 },{ 50,40 },{ 30,10 } };
	cout << solution(100, v);
}


```