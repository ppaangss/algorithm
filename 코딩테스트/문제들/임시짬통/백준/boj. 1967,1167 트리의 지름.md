
다양한 경우의 수를 생각했었다.
노드별로 가장 긴 길이를 구해놓을까도 생각했고
리프노드에서 시작도 해볼까 생각했고
아니면 각 노드별로 리프노드까지의 거리를 구해서 ??

정답은 트리에서 어느 지점에서 출발하든, 가장 멀리 있는 곳으로 달려가면 결국 그 트리의 가장자리(지름의 끝)에 도착하게 된다.

즉, 루트노드에서 제일 멀리 있는 노드 A 를 찾고, 그 노드에서 가장 멀리 있는 노드 B 를 찾는다. 이때 A와 B는 지름의 양끝점이 된다.

트리의 구현
그래프와 똑같이 구현하면 됨.

트리의 거리는 두 노드 사이의 경로는 반드시 하나만 존재하기 때문에 DFS나 BFS로 구현이 가능하다.
그래프의 경우는 아니기 때문에 최단거리 알고리즘을 사용해야 한다.

배열을 인자로 넘길 때 call by reference를 하자
call by value 하면 함수가 호출될 때 마다 인접 리스트 전체가 복사되서 메모리 초과나 시간초과날 수 있음.
아니면 전역변수로 하고 resize를 이용하기



## 풀이

- 1167

```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

// 컨테이너
#include <queue>

#define INF 87654321

using namespace std;

int MaxValue = 0;

int cal(int n, vector<vector<pair<int, int>>> v) {

    queue<int> q;
    vector<int> visited(v.size(),-1);

    q.push(n);
    visited[n] = 0;

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        for (int i = 0; i < v[node].size(); i++) {
            int nx = v[node][i].first;
            int ncost = v[node][i].second;

            if (visited[nx] == -1) {
                visited[nx] = visited[node] + ncost;
                q.push(nx);
            }
        }
    }

    int MaxIndex = max_element(visited.begin(), visited.end()) - visited.begin();
    MaxValue = *max_element(visited.begin(), visited.end());
    return MaxIndex;
    


}


int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N; cin >> N;

    vector<vector<pair<int,int>>> v(N+1);

    for (int i = 1; i <= N; i++) {
        int number = 0;
        cin >> number;

        int tmp1 = 0;
        int tmp2 = 0;
        while (1) {
            cin >> tmp1;

            if (tmp1 == -1) {
                break;
            }

            cin >> tmp2;

            v[number].push_back({ tmp1,tmp2 });

        }
    }

    int point1 = cal(1,v);
    cal(point1,v);
    
    cout << MaxValue << "\n";
}


/*

1
3 
4
2 5


*/
```