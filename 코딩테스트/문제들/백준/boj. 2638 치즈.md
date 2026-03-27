https://www.acmicpc.net/problem/2638
# 회고

상당히 어려운 시뮬레이션 + BFS 문제 였다.

문제 이해도 자체는 어렵지 않았으나, 시간이 정말 오래걸림

핵심은 외부공기를 파악하는게 더 쉬웠을 것 같다. 나는 내부 치즈공기를 파악하는게 상당히 복잡했다. 근데 그냥 0,0에서 시작한 BFS는 전부 외부공기일테니까 그거만 파악해주면 간단했을 것 같다. 

# 풀이

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

#include <queue>

using namespace std;

int dx[4] = { 0,1,0,-1 };
int dy[4] = { 1,0,-1,0 };

int N; 
int M; 
// 
vector<vector<int>> v;

// 치즈 구멍들이 잘 있는지 확인, 치즈 구멍들이 바깥에 노출되면 0으로 만들어줌.

void init() {
    vector<vector<bool>> visited(N, vector<bool>(M));

    queue<pair<int, int>> qu;

    // 0을 BFS 해서 0이랑 맞다아있는건 전부 0

    

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {

            vector<pair<int, int>> res; // 임시 저장, 2 일시 활용
            bool flag = true; // 2 인지

            if (visited[i][j] == false && v[i][j] == 0) {
                qu.push({ i,j });
                visited[i][j] = true;
                res.push_back({ i,j });
                while (!qu.empty()) {
                    int cx = qu.front().first;
                    int cy = qu.front().second;

                    qu.pop();

                    //cout << "cx-" << cx << "_cy-" << cy << "\n";

                    for (int p = 0; p < 4; p++) {
                        int nx = cx + dx[p];
                        int ny = cy + dy[p];

                        if (nx >= 0 && nx < N && ny >= 0 && ny < M) {
                            if (v[nx][ny] != 1) {
                                if (visited[nx][ny] == false) {
                                    qu.push({ nx,ny });
                                    visited[nx][ny] = true;
                                    v[nx][ny] = 0;
                                    res.push_back({ nx,ny });

                                    //cout << "nx-" << nx << "_ny-" << ny << "\n";
                                }
                            }
                        }
                        else { // 벽에 한번이라도 닿았네 그러면 그냥 공기임.
                            flag = false;
                        }
                    }
                }
            }

            //cout << "flag 합시다" << "\n";
            if (flag) {
                for (int i = 0; i < res.size(); i++) {
                    v[res[i].first][res[i].second] = 2;
                }
            }
        }
    }

    //// 디버깅
    //cout << "init" << "\n";
    //for (int i = 0; i < N; i++) {
    //    for (int j = 0; j < M; j++) {
    //        cout << v[i][j] << " ";
    //    }cout << "\n";
    //}cout << "\n";
}

void check() {

    vector<vector<bool>> visited(N, vector<bool>(M));

    queue<pair<int, int>> qu;

    // 0을 BFS 해서 0이랑 맞다아있는건 전부 0
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {

            
            if (visited[i][j] == false && v[i][j] == 0) {
                qu.push({ i,j });
                visited[i][j] = true;

                while (!qu.empty()) {
                    int cx = qu.front().first;
                    int cy = qu.front().second;

                    qu.pop();

                    for (int p = 0; p < 4; p++) {
                        int nx = cx + dx[p];
                        int ny = cy + dy[p];

                        if (nx >= 0 && nx < N && ny >= 0 && ny < M) {
                            if (v[nx][ny] != 1) {
                                if (visited[nx][ny] == false) {
                                    qu.push({ nx,ny });
                                    visited[nx][ny] = true;
                                    v[nx][ny] = 0;
                                }
                            }
                        }
                    }
                }
            }
        }
    }


    //// 디버깅
    //cout << "비어있는 치즈 공간 확인" << "\n";
    //for (int i = 0; i < N; i++) {
    //    for (int j = 0; j < M; j++) {
    //        cout << v[i][j] << " ";
    //    }cout << "\n";
    //}cout << "\n";

}

int main() {
    // 입출력 속도 최적화
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    cin >> N;
    cin >> M;

    for (int i = 0; i < N; i++) {
        vector<int> tmp;
        for (int j = 0; j < M; j++) {
            int tmpNum; cin >> tmpNum;
            tmp.push_back(tmpNum);
        }
        v.push_back(tmp);
    }

    init();

    int time1 = 0;

    while (1) {
        bool flag = true; // 치즈가 아예 없을 때까지
        vector<pair<int, int>> che;

        // 순회하면서 빈 공간의 치즈가 뚫렸는지 확인
        check();

        // 치즈 근처에 0이 몇개인지 확인 (공기와 닿은)
        for(int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {

                if (v[i][j] == 1) {
                    flag = false;
                    int cnt = 0;

                    for (int k = 0; k < 4; k++) {
                        int nx = i + dx[k];
                        int ny = j + dy[k];

                        if (v[nx][ny] == 0) {
                            cnt++;
                        }
                    }

                    if (cnt >= 2) {
                        // 없어질 치즈
                        che.push_back({ i,j });
                    }
                }
            }
        }

        // 치즈가 없어.
        if (flag) {
            break;
        }

        // 없어질 치즈 저장해놓은 벡터 순회
        for (int i = 0; i < che.size(); i++) { // 치즈 없어짐
            pair<int, int> tmp = che[i];
            v[tmp.first][tmp.second] = 0;
        }
        
        //// 디버깅
        //cout << "치즈삭제" << "\n";
        //for (int i = 0; i < N; i++) {
        //    for (int j = 0; j < M; j++) {
        //        cout << v[i][j] << " ";
        //    }cout << "\n";
        //}cout << "\n";

        // 1초끝
        time1++;
    }

    cout << time1 << "\n";

    return 0;
}

/*
8 9
0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0
0 1 1 1 0 0 1 1 0
0 1 0 1 1 1 0 1 0 
0 1 0 0 1 0 0 1 0
0 1 0 1 1 1 0 1 0
0 1 1 1 0 0 1 1 0
0 0 0 0 0 0 0 0 0 



*/
```