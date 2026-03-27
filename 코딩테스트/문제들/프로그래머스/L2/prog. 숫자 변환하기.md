




```cpp
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>
#include <queue>
#include <set>

using namespace std;

struct compare{
    bool operator()(pair<int,int> a,pair<int,int> b){
        if(a.first == b.first){
            return a.second > b.second;
        }
        return a.first > b.first;
    }
};

int solution(int x, int y, int n) {
    int answer = 0;
    
    priority_queue<pair<int,int>, vector<pair<int,int>>, compare > qu; // 숫자, 횟수
    set<int> st;
    
    qu.push({x,0});

    
    while(!qu.empty()){
        
        int num = qu.top().first;
        int cnt = qu.top().second;
        qu.pop();
        //cout << "num-" << num << "_cnt-" << cnt <<"\n";
        
        if(st.find(num) == st.end()){ // 없으면 안한거
            st.insert(num);
        }
        else{
            continue;
        }
        
        if(num == y){
            return cnt;
        }
        
        else if (num > y){
            return -1;
        }
        
        int nextCnt = cnt+1;
        
        int nextNum = num + n;
        qu.push({nextNum,nextCnt});

        nextNum = num * 2;
        qu.push({nextNum,nextCnt});
        
        nextNum = num * 3;
        //cout << "nextNum-" << nextNum << "\n";
        qu.push({nextNum,nextCnt}); 
        
        
    }

    return -1;
}
```