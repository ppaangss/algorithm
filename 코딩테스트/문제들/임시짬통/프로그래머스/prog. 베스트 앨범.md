
https://school.programmers.co.kr/learn/courses/30/lessons/42579

**분류**
- 해시
- 맵
- 정렬

해시와 맵을 이용한 구현 문제
sort 함수 3번째 인자 하는법 알아두기

# 풀이

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <unordered_map>
#include <string>

using namespace std;

vector<int> solution(vector<string> genres, vector<int> plays) {
    vector<int> answer;

    // map으로 장르별 노래횟수 저장
    unordered_map<string, int> GL; // 장르 - 노래횟수

    unordered_map<string , vector<pair<int,int>>> Songs; // 장르 - 노래들 (노래횟수, 고유번호)

    for (int i = 0; i < genres.size(); i++) {
        GL[genres[i]] += plays[i];
        Songs[genres[i]].push_back({ plays[i],i });
    }

    // 장르-노래횟수를 map에서 vector로 변경
    vector<pair<string, int>> GLMax;

    for (const auto& x : GL) {
        GLMax.push_back(x);
    }

    // 장르-노래횟수 를 노래횟수 내림차순으로 정렬
    sort(GLMax.begin(), GLMax.end(), 
        [](auto& a, auto& b) {
            return a.second > b.second;
        });

    for (const auto& x : GLMax) {
        auto s = Songs[x.first]; // 해당 장르의 노래들 가져오기

        // 노래들 노래횟수 기준으로 정렬
        sort(s.begin(), s.end(),
            [](auto a, auto b) {
                if (a.first == b.first) {
                    return a.second < b.second;
                }
                else {
                    return a.first > b.first;
                }
            });

        // 노래횟수 1,2위 노래 고유번호 저장
        answer.push_back(s[0].second);
        if (s.size() > 1) {      
            answer.push_back(s[1].second);
        }
    }
    
    return answer;
}


int main() {
    
    vector<string> v = { "classic", "pop", "classic", "classic", "pop" };
    vector<int> v1 = { 500, 600, 150, 800, 2500 };

    vector<int> result = solution(v,v1);

    for (auto x : result) {
        cout << x << " ";
    }
}

```


```
{

  "latitude": 37.4666667,

  "longitude": 126.81015

}

```