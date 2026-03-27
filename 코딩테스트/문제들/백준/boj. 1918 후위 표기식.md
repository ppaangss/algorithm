https://www.acmicpc.net/problem/1918
# 회고

이게 진정한 구현인가? 어떤 예외든 모든 경우의 수를 다 찾아야한다.

곱하기, 더하기의 우선순위, 괄호는 어떻게 처리할 것인가? 에 대한 결정이 있어야한다.

침착하게 경우의 수를 따지면서 구현을 하니까 괜찮았다.

처음에 무지성으로 시작하니까 진짜 힘들었다...

디버깅 코드를 정말 열심히 잘 써보자. 반복문 시작부터 모든 경우의 수를 확인하기 위해.

# 풀이


```cpp
#include <iostream>
#include <vector>
#include <algorithm>

#include <stack>

using namespace std;

bool check(char ch) { // 기호인가?
    
    if (ch >= 'A' && ch <= 'Z') {
        return true;
    }

    return false;
}

int main() {
    // 입출력 속도 최적화
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    string s; cin >> s;
    string res; 

    stack<char> st;


    for (int i = 0; i < s.size(); i++) {

        //cout << "i-" << i << "\n";
        
        if (check(s[i])) {
            //cout << "영어-" << s[i] << "\n";
            res += s[i];
        }
        else if (s[i] == '*' || s[i] == '/') {

            if (!st.empty()) {
                char tmp = st.top();

                if (tmp == '*' || tmp == '/') {
                    res += tmp;
                    //cout << "pop-" << tmp << "\n";
                    st.pop();
                }
            }
            //cout << "push-" << s[i] << "\n";
            st.push(s[i]);
        }

        else if (s[i] == '+' || s[i] == '-') {
            while (!st.empty()) {
                char tmp = st.top();

                if (tmp != '(') {
                    res += tmp;
                    //cout << "pop-" << tmp << "\n";
                    st.pop();
                }
                else {
                    break;
                }
            }
            //cout << "push-" << s[i] << "\n";
            st.push(s[i]);
        }

        else if (s[i] == '(') {
            //cout << "push-" << s[i] << "\n";
            st.push(s[i]);
        }

        else if (s[i] == ')') {
            while (!st.empty()) {
                char tmp = st.top();

                if (tmp != '(') {
                    res += tmp;
                    //cout << "pop-" << tmp << "\n";
                    st.pop();
                }
                else {
                    st.pop();
                    break;
                }
            }
        }
    }

    while (!st.empty()) {
        res += st.top();
        //cout << "pop-" << st.top() << "\n";
        st.pop();
    }

    cout << res << "\n";

    return 0;
}

/*

A-(B+C*D-E)/F

*/

```
