
# 

# 풀이

이분탐색

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>
#include <unordered_set>

using namespace std;

int answer = 0;

int COUNT(int s, int e, vector<int>& tp){
    unordered_set<int> us1; 
    for(int i =s;i<e;i++){
        us1.insert(tp[i]);
        //cout << tp[i] << "\n";
    }
    return us1.size();
}

void DFS(int s, int e, vector<int>& tp){
    
    if(s == e){
        //cout << "퇴장" <<"\n";
        //cout << "s-" << s << " e-" << e << "\n";
        
        return;
    }
    
    int mid = (s + e) / 2;

    int cnt1 = COUNT(0,mid,tp);
    int cnt2 = COUNT(mid,tp.size(),tp);
    
    if(cnt1 == cnt2){ // 공평한 것이다.
        // cout << "s-" << s << " e-" << e << "\n";
        // cout << "us1-" << us1.size() << " us2-" << us2.size() << "\n";
        answer++;
        // DFS(s,mid,tp);
        // DFS(mid+1,e,tp);
    }
    else if(cnt1 > cnt2){
        DFS(s,mid,tp);
    }
    else if(cnt1 < cnt2){
        DFS(mid+1,e,tp);
    }
}

int solution(vector<int> topping) {
    
    DFS(0,topping.size(),topping);
    
    return answer;
}
```