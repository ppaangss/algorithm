https://school.programmers.co.kr/learn/courses/30/lessons/135807
# 회고

최대공약수를 구하면 쉬운 문제였는데 처음에 다르게 접근했다.

배열 A 에서 가장 작은 숫자에 대한 약수들을 구한다.

그래서 그 약수들을 가지고 배열 A에서 나뉘는지, 배열 B에서 안나뉘는지 확인해서 구하는 방식이다.

시간복잡도를 고려를 해봤는데 아슬아슬했다.

- `min_element`로 배열의 최솟값 찾기: $O(N)$
- 최솟값의 모든 약수 구하기: $O(\sqrt{M})$ ($M$은 배열 원소의 크기, 최대 1억)
- 약수 정렬: $O(D \log D)$ ($D$는 약수의 개수)
- 모든 약수에 대해 배열 전체 순회: $O(D \times N)$

여기서 $O(D \times N)$ 가 위험했다. 대략 약수의 개수를 몰라서 일단 풀었었는데 약수의 최악의 개수는 대략 100~500 정도이다. 그러면 $O(D \times N)$ = 250,000,000 으로 시간초과가 나게 된다.

물론, 이렇게 해서 해결을 하긴 했지만 시간복잡도 측면에서 아쉬운 문제였다.

최대공약수를 통해 풀면 간단하게 해결 가능하다. 

# 풀이

**아쉬운 풀이**

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

int solution(vector<int> arrayA, vector<int> arrayB) {
    int answer = 0;
    
    //cout << "A" << "\n";
    int minA = *min_element(arrayA.begin(),arrayA.end());
    
    //cout << minA << "\n";
    
    // 약수 찾기
    vector<int> v; // 약수 집합
    for(int i = 1;i * i <= minA; i++){
        if(minA % i == 0){
            v.push_back(i);
            v.push_back(minA / i);
        }
    }
    sort(v.begin(),v.end(),[](auto a,auto b){
        return a > b;
    });
    
    // cout << "x-";
    // for(auto x : v){
    //     cout << x << " ";
    // }cout << "\n";
    
    // 순회
    bool flagA = true; // 성공했는가?
    
    for(int k = 0;k<v.size();k++){
        //cout << "v[k]" << v[k] << "\n";
        flagA = true;
        for(int i =0;i<arrayA.size();i++){
            if(arrayA[i] % v[k] != 0){ // 나눠떨어지지 않음.
                flagA = false;
                break;
            }
        }

        for(int i =0;i<arrayB.size();i++){
            if(arrayB[i] % v[k] == 0){ // 나눠떨어짐.
                flagA = false;
                break;
            }
        }
        
        if(flagA == true){
            if(answer < v[k]){
                answer = v[k];
                //cout << "answer-"<< answer << "\n";
                break;
            }
        }
    }
    
    // 배열 B
    //cout << "B" << "\n";
    
    int minB = *min_element(arrayB.begin(),arrayB.end());
    
    //cout << minA << "\n";
    
    // 약수 찾기
    vector<int> v2; // 약수 집합
    for(int i = 1;i * i <= minB; i++){
        if(minB % i == 0){
            v2.push_back(i);
            v2.push_back(minB / i);
        }
    }
    
    sort(v2.begin(),v2.end(),[](auto a,auto b){
        return a > b;
    });
    
    bool flagB = true; // 성공했는가?
    
    for(int k = 0;k<v2.size();k++){
        //cout << "v[k]" << v2[k] << "\n";
        flagB = true;
        for(int i =0;i<arrayB.size();i++){
            if(arrayB[i] % v2[k] != 0){ // 나눠떨어지지 않음.
                flagB = false;
                break;
            }
        }

        for(int i =0;i<arrayA.size();i++){
            if(arrayA[i] % v2[k] == 0){ // 나눠떨어짐.
                flagB = false;
                break;
            }
        }
        
        if(flagB == true){
            if(answer < v2[k]){
                answer = v2[k];
                //cout << "answer-"<< answer << "\n";
                break;
            }
        }
    }
    
    return answer;
}
```

**최대공약수 풀이**

```cpp
#include <string>
#include <vector>
#include <numeric>
#include <algorithm>

using namespace std;

// 배열의 모든 요소에 대한 최대공약수를 구하는 함수
int get_array_gcd(const vector<int>& arr) {
    int res = arr[0];
    for (size_t i = 1; i < arr.size(); ++i) {
        res = gcd(res, arr[i]);
    }
    return res;
}

// 특정 숫자가 배열의 어떤 원소도 나누지 못하는지 확인하는 함수
bool cannot_divide_any(int a, const vector<int>& arr) {
    for (int num : arr) {
        if (num % a == 0) return false;
    }
    return true;
}

int solution(vector<int> arrayA, vector<int> arrayB) {
    int answer = 0;
    
    // 1. 각 배열의 최대공약수 구하기
    int gcdA = get_array_gcd(arrayA);
    int gcdB = get_array_gcd(arrayB);
    
    // 2. Case A 확인 (gcdA가 arrayB를 나누지 못하는지)
    if (cannot_divide_any(gcdA, arrayB)) {
        answer = max(answer, gcdA);
    }
    
    // 3. Case B 확인 (gcdB가 arrayA를 나누지 못하는지)
    if (cannot_divide_any(gcdB, arrayA)) {
        answer = max(answer, gcdB);
    }
    
    return answer;
}
```