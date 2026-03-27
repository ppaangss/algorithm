https://school.programmers.co.kr/learn/courses/30/lessons/169199#
# 회고

2차원 배열 인덱스 조절이 제일 어렵다 슈발람

어떤 루틴을 정해놔야하나?

그리고 이제 dx dy 순서를 어떻게 하는게 좋은지 검토해보자.


# 풀이


```cpp
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

int dx[4] = {0,1,0,-1};
int dy[4] = {1,0,-1,0};

struct point{
    int x;
    int y;
    int cnt;
};

int solution(vector<string> board) {
    int answer = 0;
    
    int n = board.size();
    int m = board[0].size();
    
    //cout << n << m << "--\n";
    
    vector<vector<char>> bod(n,vector<char>(m));
    vector<vector<bool>> visited(n,vector<bool>(m)); // 방문함수
    
    point start;
    point end;
    
    for(int i =0;i<n;i++){
        for(int j = 0;j<m;j++){
            bod[i][j] = board[i][j];
            
            if(board[i][j] == 'R'){
                start = {i,j,0};
            }
            
            if(board[i][j] == 'G'){
                end = {i,j,0};
            }
        }
    }
    
//     //디버깅 코드

    
//     for(int i =0;i<n;i++){
//         for(int j = 0;j<m;j++){
//             cout << bod[i][j] << " ";
//         }
//         cout << "\n";
//     }
    
    queue<point> qu;
    
    qu.push(start);
    visited[start.x][start.y] = true;
    
    while(!qu.empty()){
        
        int cx = qu.front().x;
        int cy = qu.front().y;
        int curCnt = qu.front().cnt;
        
        if(cx == end.x && cy == end.y){
            return curCnt;
        }
        
        //cout << "cx-" << cx << "_cy-" << cy << "_cur-" << curCnt << "\n";
        
        qu.pop();
        
        for(int i =0;i<4;i++){
            
            int nx = cx;
            int ny = cy;
            
            if(dx[i] == 0){ // 위아래로 움직임 
                while(1){
                    if(!(ny + dy[i] >= 0 && ny + dy[i] < m)){ // 벽인 경우 종료
                        break;
                    }
                    if(bod[nx][ny + dy[i]] == 'D'){ // 벽인경우 종료
                        break;
                    }
                    ny += dy[i];
                }
            }
            else { //좌우로 움직임
                while(1){
                    if(!(nx + dx[i] >= 0 && nx + dx[i] < n)){ // 벽인 경우 종료
                        break;
                    }
                    if(bod[nx + dx[i]][ny] == 'D'){ // 벽인경우 종료
                        break;
                    }
                    nx += dx[i];
                }
            }
            //cout << "nx-" << nx << "_ny-" << ny << "\n";
            if(visited[nx][ny] == false){
                //cout << "nx-" << nx << "_ny-" << ny << "\n";
                qu.push({nx,ny,curCnt+1});
                visited[nx][ny] = true;
            }
        } 
    }
    
    return -1;
}
```