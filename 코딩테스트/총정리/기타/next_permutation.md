
# next_permutation 쓰는법

기억이 안나면 이거 봐라

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // lower_bound, upper_bound 포함
#include <string>

using namespace std;

int main() {

    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    int N; cin >> N;

    vector<int> v(N);
    for (int i = 0; i < N; i++) {
        v[i] = i + 1;
    }

    do {

        for (int i = 0; i < v.size(); i++) {
            cout << v[i] << " ";
        }cout << "\n";


    } while (next_permutation(v.begin(), v.end()));

    return 0;
}

```