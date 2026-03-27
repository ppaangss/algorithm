https://school.programmers.co.kr/learn/courses/30/lessons/134239
# 회고

문제 자체는 그냥 따라가면서 풀기 쉬웠다.

간단히 복습할게 어떤 구간을 구해야 할 경우 누적합을 항상 고민하도록 하자.

만약 범위가 크면 누적합을 써야 풀리는 문제가 생길 수 있다.

그리고 나눌 때 자료형 주의하자.

`double num = (a + b) / 2 ;`

int a와 b를 더해 나눌 때 double 로 넣을 때 형변환을 안해주면 int 값만 들어간다.
따라서 다음과 같이 해줘야함.

`double num = (a + b) / 2.0 ;`

`double num = (double) (a + b) / 2 ;`

# 풀이

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

vector<double> solution(int k, vector<vector<int>> ranges) {
    vector<double> answer;
    
    vector<int> vt;
    vt.push_back(k);
    
    while(k > 1){
        if(k % 2 == 0){
            k = k / 2;
        }
        else{
            k = k * 3 + 1;
        }
        
        vt.push_back(k);
    }
    
    // // 디버깅
    // for(auto x : vt){
    //     cout << x << "\n";
    // }
    
    // 0이면 0~1의 정적분
    vector<double> wd;
    
    for(int i =0;i<vt.size()-1;i++){
        double tmp = (double)(vt[i] + vt[i+1]) / 2.0;
        wd.push_back(tmp);
        
        //cout << tmp << "\n";
    }
    
    for(int i =0;i<ranges.size();i++){
        int s1 = ranges[i][0]; 
        int s2 = ranges[i][1];
        s2 += vt.size()-1;
        
        double sum = 0;
        for(int i =s1;i<s2;i++){
            sum += wd[i];
        }
        
        if(s1 > s2){
            answer.push_back(-1);
            //cout << -1 << "\n";
        }
        else{
            answer.push_back(sum);
            //cout << sum << "\n";
        }
    }
    
    return answer;
}

```