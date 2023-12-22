## 유니온 파인드(Union-Find)
**`#그래프`** **`#Disjoint-Set`** **`#분리 집합`** **`#서로소 집합`** **`#상호베타적 집합`**   

![유니온 파인드](https://i.imgur.com/1tr43yL.png)

**- 두 노드(정점)가 같은 집합에 속하는지 판별하는 알고리즘**
- 각 노드의 링크가 **`부모 노드`** 쪽으로 향하는 것이 특징
- **`루트 노드`** 는 자식 노드들이 속한 **`집합의 대푯값`** 으로 표현
- 한 노드는 **둘 이상의 집합에 소속되지 않음**
<br>

## 구현

1. **`부모 노드를 저장하는 배열(parents)`을 선언**
- 각자 자기 자신으로 초기화
- **`parents[i] = i`** 는 루트 노드임을 의미
```cpp
#include <vector>

int size = 10;
vector<int> parents(size);

for (int i = 0; i < size; i++)
	parents[i] = i;
```
<br>

2. **`Find(x)` 함수 구현**
- 주어진 원소 **`x`** 가 어느 집합에 속하는지 찾는 연산
- 자신이 속한 집합을 알려면, 현재 노드에서 루트 노드를 찾아 들어가야 함
- **`x == parents[x]`**  는 루트 노드를 의미하니, 그대로 리턴
- 그렇지 않다면, 재귀적으로 루트 노드를 찾아 리턴
```cpp
int Find(int x) {
	if (x == parents[x])
		return x;

	/* 루트 노드를 찾았다면, 현재 노드의 부모를 루트 노드로 변경 */
	int root = find(parents[x]);
	parents[x] = root;
	return root;
}
```
<br>

3. **`Union()` 함수 구현**
- 서로 다른 두 개의 집합을 하나로 합치는 연산
- **`높이가 낮은 트리`** 가 **`높이가 높은 트리`** 의 자식으로 들어가야 편향 트리가 만들어지지 않음
- 이를 위해, **`트리의 높이를 기억하는 배열(rank)`** 이 필요
```cpp
// 초기값 높이는 1로 지정

vector<int> rank(size)
for (int i = 0; i < size; i++)
	rank[i] = 1;
```
```cpp
void Union(int x, int y) {
    int xRoot = Find(x);
    int yRoot = Find(y);

	if (xRoot == yRoot)
	    return;
	    
    // 두 집합의 높이가 같은 경우에는 y를 x의 자식으로 넣는 걸로 정하였음
    // 이럴 경우, 전체 트리의 높이는 "x의 높이 + 1"이 됨
    if (rank[xRoot] == rank[yRoot]) {
	    parent[yRoot] = xRoot;
        rank[xRoot] += 1;
    }

    else if (rank[xRoot] < rank[yRoot])
        parent[xRoot] = yRoot;
    else
        parent[yRoot] = xRoot;
}
```
<br>

4. **위 기능들을 총 집합해 작성한 `DisjointSet` 클래스**
```cpp
#include <vector>
using namespace std;

template<typename T>
class DisjointSet {
public:
    DisjointSet(int size) : parent(size), rank(size) {
        for (int i = 0; i < size; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

public:
    T Find(const T& element) {
        if (element == parent[element])
            return T;
            
        T root = Find(parent[element]);
        parent[element] = root;
        return root;
    }

    void Union(const &T x, const T& y) {
        T xRoot = Find(x);
        T yRoot = Find(y);

        if (xRoot == yRoot)
            return;

        // 두 집합의 높이가 같은 경우에는 y를 x의 자식으로 넣는 걸로 정하였음
        // 이럴 경우, 전체 트리의 높이는 "x의 높이 + 1"이 됨
        if (rank[xRoot] == rank[yRoot]) {
            parent[yRoot] = xRoot;
            rank[xRoot] += 1;
        }
        
        else if (rank[xRoot] < rank[yRoot])
            parent[xRoot] = yRoot;

        else
            parent[yRoot] = xRoot;
    }
    
    inline bool IsSame(const T& x, const T& y) { return Find(x) == Find(y); }

private:
    vector<T> parent;
    vector<T> rank;
};
```
