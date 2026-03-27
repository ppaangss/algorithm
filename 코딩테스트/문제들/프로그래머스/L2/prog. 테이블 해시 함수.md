https://school.programmers.co.kr/learn/courses/30/lessons/147354
# 회고

딱히 어렵지는 않은 문제였다.

대신 정렬할 떄 람다함수를 이용해 구현하는 거 왜 안됐는지 모르는데 다시하니까 잘 됨..

처음으로 람다 함수 내에 비교 조건이 외부 변수를 사용해야 하는 문제가 나왔다. 그래서 캡처를 이용해 풀 수 있었다.

근데 만약에 그냥 bool 함수로 비교할거면 그냥 전역변수 써야할듯. 다른 방식있는데 너무 어렵다. 모르는거다 그냥.

# 풀이

```cpp
#include <string>
#include <vector>
#include <iostream>
#include <algorithm>

using namespace std;

// int coltmp = 0;
// bool compare(vector<int> a, vector<int> b){
//      if(a[coltmp-1] == b[coltmp-1]){
//             return a[0] > b[0];
//         }
//     return a[coltmp-1] < b[coltmp-1];
// }
int solution(vector<vector<int>> data, int col, int row_begin, int row_end) {
    int answer = 0;
    //coltmp = col;
    
    int N = data.size();
    int M = data[0].size();
    
    sort(data.begin(),data.end(),[col](vector<int> a, vector<int> b){
        if(a[col-1] == b[col-1]){
            return a[0] > b[0];
        }
        return a[col-1] < b[col-1];
    });
    
    // // 디
    // for(int i =0;i<N;i++){
    //     for(int j =0;j<M;j++){
    //         cout << data[i][j] << " ";
    //     }cout << "\n";
    // }
    
    vector<int> res;
    
    // 2 3
    for(int i =row_begin;i<=row_end;i++){
        int sum = 0;
        for(int j =0;j<M;j++){
            sum += ( data[i-1][j] % (i));
        }
        res.push_back(sum);
        //cout << sum << "\n";
    }
    
    int res1 = res[0];
    for(int i =1;i<res.size();i++){
        res1 = res1 ^ res[i];
    }

    return res1;
}
```