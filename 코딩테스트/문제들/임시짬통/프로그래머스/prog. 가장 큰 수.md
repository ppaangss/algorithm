
https://school.programmers.co.kr/learn/courses/30/lessons/42746

문자로된 숫자의 비교는 앞자리 부터의 숫자 비교에 사용할 수 있음.

문자의 경우
"12" 와 "123" 의 경우 "123"이 더 큼
"13" 과 "123"의 경우 "13"이 더 큼
숫자의 경우
"13"와 "123"의 경우 "123" 이 더 큼

이 문제의 경우 비교 방법 알고리즘이 중요했다.
문자 비교 방법을 앞 + 뒤 vs 뒤 + 앞 으로 하는 것이 중요했다.

예외케이스를 항상 잘 해야하겠다고 생각했다. 이번 문제의 경우 0 0 0 이 들어올 경우 답이 000 이 아닌 0 이어야 한다는 예외가 있었다.

# 풀이

```c++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(vector<int> numbers) {
    vector<string> Nums;

    for (int x : numbers) {
        Nums.push_back(to_string(x));
    }

    sort(Nums.begin(), Nums.end(),
        [](const string& a, const string& b) {
            return a + b > b + a;
        });

    // 모두 0인 경우 처리
    if (Nums[0] == "0") return "0";

    string answer;
    for (const string& x : Nums) {
        answer += x;
    }

    return answer;
}


int main() {
    // vector<vector<int>> commands = {{2, 5, 3}, {4, 4, 1}, {1, 7, 3}};

    vector<int> array = { 0, 0 ,0 };
    
    //for (auto x : solution(array)) {
    //    cout << x << "\n";
    //}
    cout << solution(array);
}
```