# map은 정렬을 할 수 없다.

백준 20920 영단어 암기는 괴로워 를 풀고 있을 때였다.

unordered_map을 썼는데 여기 있는 데이터를 가지고 정렬을 해야했다.

근데 map은 정렬이 안된다. 왜냐하면 map은 key 기준 자동 정렬이기 때문에 임의로 정렬할 수 없다.

그래서 map을 vector 로 옮긴다음 정렬을 해야한다.

# map을 vector 로 옮기기

map도 사실 내부적으로는 `pair<타입,타입>` 형태를 가진다. 

따라서 `vector<pair<타입,타입>>` 에 옮길 수 있다.

```cpp
unordered_map<string,int> um;

vector<pair<string, int>> vt;

for (auto x : um) {
	vt.push_back(x);
}
```


# 정렬이 가능한 자료구조

벡터 뿐만 아니라 데크도 가능하다.