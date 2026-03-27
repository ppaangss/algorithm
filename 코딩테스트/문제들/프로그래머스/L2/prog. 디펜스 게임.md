https://school.programmers.co.kr/learn/courses/30/lessons/142085
# 회고

우선순위 큐를 순수하게 프로그래머스 내에서 구현할 수 있도록 연습해야한다.

일단, 우선순위 큐 철자도 기억이 잘 안났고, 사용자 정렬 구조체 만드는거 못했다. 이거 복습!!!!

# 해결과정

무난한 그리디 문제

우선순위 큐를 활용하면 쉽게 풀리는 문제였다.



# 풀이

```cpp
#include <string>
#include <vector>
#include <iostream>
#include <queue>

using namespace std;

struct Comapre {
    bool operator() (int a, int b) {
        return a > b;
    }
};

int solution(int n, int k, vector<int> enemy) {
    int answer = 0;

    int sum = 0; // 무적권 제외하고 받아야하는 적의 수

    priority_queue<int, vector<int>, Comapre> pq;
    
    for (int i = 0; i < k; i++) {
        pq.push(enemy[i]);
    }
    
    cout << pq.top() << "\n";

    for (int i = k; i < enemy.size(); i++) {
        //cout << enemy[i] << "-";
        if(pq.top() < enemy[i]){
            pq.push(enemy[i]);
            sum += pq.top();
            pq.pop();
        }
        else{
            sum += enemy[i];
        }
        //cout << sum << ":";
        
        if(sum > n){ //
            return i;
        }
    }
    
    return enemy.size();
}


/*

enemy 배열중 하나다.!! 1000000
정렬 O(logN)

최소값 , 1개

*/

```





