https://www.acmicpc.net/problem/17144
# 회고

문제 이해 + 순수 구현의 실력 싸움이었다...

어렵지는 않았는데 그냥 문제이해하고 코드짜는데 1시간이 걸림. 

이런거 시간을 단축시킬수가 있을까? 라는 의문이 든다.


# 풀이


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

#include <queue>

using namespace std;

struct pt {
    int x;
    int y;
    int num;
};

int dx[4] = { 0,1,0,-1 };
int dy[4] = { 1,0,-1,0 };


int main() {
    // 입출력 속도 최적화
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int R; cin >> R;
    int C; cin >> C;
    int T; cin >> T;

    int gx = 0; // 공기청정기
    int gy = 0;
    queue<pt> qu;

    for (int i = 0; i < R; i++) {
        for (int j = 0; j < C; j++) {
            int tmp; cin >> tmp;
            if (tmp > 0) {
                qu.push({i,j,tmp});
            }
            else if (tmp == -1) { // 공기청정기 좌표

                if (!(gx == 0 && gy == 0)) {
                    continue;
                }

                gx = i;
                gy = j;
            }
        }
    }

    for (int x = 0; x < T; x++) {

        //cout << "x-" << x << "\n";
        
        vector<vector<int>> map(R, vector<int>(C, 0));

        while (!qu.empty()) {

            pt cur = qu.front();

            qu.pop();

            int cx = cur.x;
            int cy = cur.y;
            map[cx][cy] += cur.num;
            int num = cur.num / 5;

            //// 디버깅
            //cout << "cx-" << cx << "_cy-" << cy << "\n";
            //cout << "num" << cur.num << "\n";

            for (int i = 0; i < 4; i++) {
                
                int nx = cur.x + dx[i];
                int ny = cur.y + dy[i];

                if (nx >= 0 && nx < R && ny >= 0 && ny < C) { // 미세먼지 확산

                    if (ny == gy) { // 공기청정기 좌표
                        if (nx == gx || nx == gx + 1) {
                            continue;
                        }
                    }
                    map[cx][cy] -= num;
                    map[nx][ny] += num;
                }
            }
        }

        //// 디버깅
        //for (int i = 0; i < R; i++) {
        //    for (int j = 0; j < C; j++) {
        //        cout << map[i][j] << " ";
        //    }cout << "\n";
        //}cout << "\n";

        //// 바람

        // 공기청정기 위
        map[gx][gy] = 0;
        map[gx+1][gy] = 0;
        for (int i = gx-1; i > 0; i--) {
            map[i][gy] = map[i - 1][gy];
        }

        // 공기 청정기 아래
        for (int i = gx + 2; i < R-1; i++) {
            map[i][gy] = map[i + 1][gy];
        }

        // 맨 위
        for (int i = 0; i < C - 1; i++) {
            map[0][i] = map[0][i + 1];
        }

        // 맨 아래
        for (int i = 0; i < C - 1; i++) {
            map[R-1][i] = map[R-1][i + 1];
        }

        // 오른쪽 0 1 
        for (int i = 0; i < gx; i++) {
            map[i][C-1] = map[i + 1][C - 1];
        }

        // 오른쪽 5 4 3
        for (int i = R-1; i > gx; i--) {
            map[i][C-1] = map[i - 1][C-1];
        }

        // 가운데 6 5 4 3 2 1
        for (int i = C-1; i > 0; i--) {
            map[gx][i] = map[gx][i - 1];
            map[gx+1][i] = map[gx+1][i - 1];
        }

        //// 디버깅
        //for (int i = 0; i < R; i++) {
        //    for (int j = 0; j < C; j++) {
        //        cout << map[i][j] << " ";
        //    }cout << "\n";
        //}cout << "\n";

        

        // 현 미세먼지 큐에 넣기
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                int tmp = map[i][j];
                if (tmp > 0) {
                    qu.push({ i,j,tmp });
                }
            }
        }

    }

    int sum = 0;

    while (!qu.empty()) {
        sum += qu.front().num;
        qu.pop();
    }

    // 큐에 있는 미세먼지 합산
    cout << sum << "\n";

    return 0;
}

/*




*/



```