https://school.programmers.co.kr/learn/courses/30/lessons/131127#
- map
- 슬라이딩 윈도우
# 회고




# 풀이

```cpp
#include <string>
#include <vector>
#include <unordered_map>
#include <iostream>

using namespace std;

bool CHECK(vector<string>& want, vector<int>& number, unordered_map<string,int>& um){
    
    for(int i =0;i<want.size();i++){
        
        //cout << "과일-" << want[i] << "\n";
        //cout << "개수-" << number[i] << "\n";
        if(um.count(want[i])){
            //cout << "개수-" << um[want[i]] << "\n";
            if(um[want[i]] != number[i]){
                return false;
            }
        }
        else{
            return false;
        }
    }
    
    return true;
}

int solution(vector<string> want, vector<int> number, vector<string> discount) {
    int answer = 0;
    
    int size1 = 0;
    for(int i =0;i<number.size();i++){
        size1 += number[i];
    }
    
    unordered_map<string,int> um;
    
    for(int i =0;i<size1;i++){
        um[discount[i]]++;
    }
    
    // for(auto x : um){
    //     cout << "상품-" << x.first << " 개수-" << x.second << "\n";
    // }cout << "\n";
    
    if(CHECK(want,number,um)){ // 첫날이면 끝
        answer++;
    }
    
    for(int i =0;i< discount.size() - size1;i++){
        //cout << "날짜-" << i+1 << "\n";
        
        um[discount[i]]--;
        um[discount[i+size1]]++;
        
        // for(auto x : um){
        //     cout << "상품-" << x.first << " 개수-" << x.second << "\n";
        // }
        
        if(CHECK(want,number,um)){
            answer++;
            //cout << "check" << "\n";
        }
        
        //cout << "\n";
    }

    return answer;
}

/*

	["apple", "banana", "apple", "banana", "apple"]


*/
```