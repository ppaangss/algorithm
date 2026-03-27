
https://www.acmicpc.net/problem/1504

최단경로 문제
방향이 없는 그래프, 한번 이동했던 간선 다시 이용 가능

다익스트라 구현 방식 익혀놓기
오버플로우 조심 
long long 항상 생각하기



## 풀이

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

// 컨테이너
#include <unordered_map>
#include <queue>

#define INF 87654321

using namespace std;

struct graph {
    int end;
    int dis;
};

struct compare {
    bool operator()(pair<int, int> a, pair<int, int> b) {
        return a.first < b.first;
    }
};



vector<int> dj(int n,int N, vector<vector<graph>> v) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, compare> pq; // first: 거리, second: 숫자
    vector<int> visited(N + 1, INF);

    pq.push({ 0,n });
    visited[n] = 0;
    
    while (!pq.empty()) {
        // 가장 작은 거리에 대한 값 추출
        int CurNum = pq.top().second;
        int CurDis = pq.top().first;
        pq.pop();

        if (visited[CurNum] < CurDis) {
            continue;
        }

        // 가장 작은 거리 확정
        visited[CurNum] = CurDis;

        // 해당 노드에 연결되어 있는 노드들 큐에 넣기
        for (int i = 0; i < v[CurNum].size(); i++) {
            int tmpNum = v[CurNum][i].end;
            int tmpDis = v[CurNum][i].dis;
            tmpDis += CurDis;

            // 연결되어 있는 노드들의 거리가 작은 것만 큐에 넣기
            if (tmpDis < visited[tmpNum]) {
                visited[tmpNum] = tmpDis;
                pq.push({ tmpDis,tmpNum });
            }
        }
    }

    return visited;
}



int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);

    int N; cin >> N;
    int E; cin >> E;

    vector<vector<graph>> v(N + 1);
    

    for (int i = 0; i < E; i++) {
        int tmp1, tmp2, tmp3;
        cin >> tmp1 >> tmp2 >> tmp3;

        v[tmp1].push_back({ tmp2,tmp3 });
        v[tmp2].push_back({ tmp1,tmp3 });
    }

    int v1, v2;
    cin >> v1 >> v2;

    vector<int> result1 = dj(1, N, v);
    vector<int> result2 = dj(v1, N, v);
    vector<int> result3 = dj(v2, N, v);

    long long p1 = (long long)result1[v1] + result2[v2] + result3[N];
    long long p2 = (long long)result1[v2] + result3[v1] + result2[N];


    long long res = min(p1, p2);

    if (res >= INF) {
        res = -1;
    }

    cout << res << "\n";
}


/*


1 -> v1 -> v2 -> N
1 -> v2 -> v1 -> N

*/
```