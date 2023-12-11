#그래프 #최소신장트리 #그리디 #유니온파인드 


## 크루스칼 알고리즘(Kruskal's Algorithm)
- 가중치 그래프에서 **`탐욕법(Greedy)`** 방식으로 최소 신장 트리를 구축하는 방법
- 모든 간선 정보를 사전에 파악 후, 하나씩 구축해 나가는 것이 특징
- 간선을 기반으로 하다 보니, **간선의 개수가 적을 때 효율적**


### 알고리즘 동작 과정
1. **그래프 내의 모든 간선들에 대해 가중치를 기준으로 오름차순으로 정렬**
2. **정렬한 간선을 차례대로 순회하며, 최소 신장 트리에 간선 추가**
   - 단, 최소 신장 트리 내에 사이클을 형성하는 간선은 제외
   - [**유니온 파인드(Union-Find)**](유니온%20파인드(Union-Find).md) 알고리즘을 통해 사이클을 형성하는 간선 검사
3. **모든 간선에 대해 2번 과정을 반복**
<br>

### 소스 코드

```cpp
#include <vector>
#include <algorithm>
using namespace std;

struct Edge {
    Edge() { }
    Edge(int node1, int node2, int cost) : node1(node1), node2(node2), cost(cost) { }
    bool operator<(const Edge& other) const { return cost < other.cost; }
    
    int node1, node2, cost;
};

class MST {
public:
    MST(const vector<Edge>& edgeInfo)
    : edges(edgeInfo.begin(), edgeInfo.end()), parent(edgeInfo.size()), rank(edgeInfo.size()) {
        for (int i = 0; i < edgeInfo.size(); i++) {
            parent[i] = i;
            rank[i] = 1;
        }
        
        sort(edges.begin(), edges.end());
    }

    int GetCost() {
        int totalCost = 0;
        
        for (const auto& edge : edges) {
            if (IsSame(edge.node1, edge.node2))
                continue;

            Union(edge.node1, edge.node2);
            totalCost += edge.cost;
        }

        return totalCost;
    }

private:
    int Find(int x) {
        if (x == parent[x])
            return x;
        
        int root = Find(parent[x]);
        parent[x] = root;
        return root;
    }

    void Union(int x, int y) {
        int xRoot = Find(x), yRoot = Find(y);
        
        if (xRoot == yRoot)
            return;

        if (rank[xRoot] == rank[yRoot]) {
            parent[yRoot] = xRoot;
            rank[xRoot] += 1;
        }

        else if (rank[xRoot] < rank[yRoot])
            parent[xRoot] = yRoot;
        else
            parent[yRoot] = xRoot;
    }

    bool IsSame(int x, int y) { return Find(x) == Find(y); }

private:
    vector<Edge> edges;
    vector<int> parent;
    vector<int> rank;
};
```
<br>

## 성능 분석
- 정렬 알고리즘 + 유니온 파인드 알고리즘 시간 복잡도 합이므로, 사실상 **정렬 알고리즘의 시간 복잡도와 동일**하다고 판단해도 큰 무리가 없음
