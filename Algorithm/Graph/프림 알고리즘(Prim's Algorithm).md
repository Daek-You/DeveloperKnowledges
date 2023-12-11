## 프림 알고리즘(Prim's Algorithm)
**`#그래프`** **`#최소신장트리`**  **`#Greedy`**   

- [**크루스칼 알고리즘**](./크루스칼%20알고리즘(Kruskal's%20Algorithm).md)과 같이, 최소 신장 트리를 구축하는 또 하나의 탐욕법 기반 알고리즘
- 한 정점을 시작으로, 인접한 정점들 중 가장 작은 가중치의 정점을 선택해 나가는 방식
- 정점을 기반으로 하다 보니, **간선의 개수가 많을 때, 효율적**
- 이미 선택됐던 정점들과 인접한 정점들 중에서 고르는 방식이기에 트리 형태가 유지됨  
  👉🏻 즉, 사이클을 형성하지 않는 방법이기에 [**유니온 파인드**](유니온%20파인드(Union-Find).md)와 같은 알고리즘 사용 X
<br>

### 알고리즘 동작 과정

1. **그래프에서 `임의의 정점 하나` 를 정해, `최소 신장 트리 집합(T)`에 넣는다.**  
2. **`트리에 속한 정점들`과 `인접한 정점들` 중에 `가장 작은 가중치로 연결된 정점`을 `T`에 넣는다.**
	- 가장 작은 가중치의 정점을 찾는 과정에서 **`우선순위 큐(Priority Queue)`** 사용
	- 만약 해당 정점이 이미 T에 속해 있다면(사이클을 형성한다면),
	  다음으로 작은 가중치의 정점을 T에 넣는다.
3. **T의 집합 개수가 그래프의 정점 개수와 같아질 때까지, 2번 과정을 반복한다.**  
<br>

### 소스 코드

```cpp
#include <vector>
#include <queue>
using namespace std;
using Data = pair<int, int>  // <비용, 정점>

int Prim(const vector<vector<Data>>& graph, int startVertex) {

    // 1. 시작 정점을 최소 신장트리 집합(T)에 넣기
    vector<bool> T(graph.size(), false);
    T[startVertex] = true;

    // 2. 시작 정점과 인접한 다른 정점 정보 우선순위 큐에 넣기
    priority_queue<Data, vector<Data>, greater<Data>> candidateVertexes;
    for (const auto& adjacentVertex : graph[startVertex]) {
    
        int cost = adjacentVertex.first;
        int next = adjacentVertex.second;
        candidateVertexes.emplace(cost, next);
    }

    int answer = 0;
    while (!candidateVertexes.empty()) {
    
        int nextCost = candidateVertexes.top().first;
        int nextVertex = candidateVertexes.top().second;
        candidateVertexes.pop();
        
        // 이미 최소 신장 트리 집합(T)에 속한 정점이라면, 사이클을 형성하므로 X
        if (T[nextVertex])
            continue;

        // 3. 인접 정점들 중에 가장 낮은 가중치를 가진 정점을 T에 넣기
        T[nextVertex] = true;
        answer += nextCost;

        // 4. 인접한 정점 정보 우선 순위 큐에 넣기
        for (const auto& adjacentVertex : graph[nextVertex]) {

            int candidateCost = adjacentVertex.first;
            int candidateVertex = adjacentVertex.second;
            candidateVertexes.emplace(candidateCost, candidateVertex);
        }
    }

    return answer;
}

```  

🔗[**BOJ 1922 - 네트워크 연결**](https://www.acmicpc.net/problem/1922)  
```cpp
int main() {

	int computerCount, connectableDataCount;
    cin >> computerCount >> connectableDataCount;

    vector<vector<Data>> adjacentComputers(computerCount + 1, vector<Data>());
    int nodeA, nodeB, cost;
    for (int i = 0; i < connectableDataCount; i++) {

        cin >> nodeA >> nodeB >> cost;
        adjacentComputers[nodeA].emplace_back(cost, nodeB);
        adjacentComputers[nodeB].emplace_back(cost, nodeA);
    }

    int answer = Prim(adjacentComputers, computerCount);
    cout << answer;
	return 0;
}
```

<br>

## 성능 분석

### 우선순위 큐를 사용한 경우

> **정점의 개수 : $V$ , 간선의 개수 : $E$**  

$$ \mathrm{O((E  + V) \ logV)}$$  <br>

1. 매 정점마다 최소 가중치의 간선을 찾는 데에 $\mathrm{O(logV)}$  
	- 따라서, 모든 정점에 대해 최소 가중치 간선 탐색 시간은 $\mathrm{O(V \ logV)}$  
2. 각 정점의 인접 정점 간선을 찾는 시간은 모든 정점의 차수와 같으므로 $\mathrm{O(2E) = \mathrm{O(E)}}$  
	- 찾은 인접 정점 간선을 우선순위 큐에 넣는 데에 $\mathrm{O(logV)}$ 이 소모
	- 따라서, 총 걸리는 시간은 $\mathrm{O(E \ logV)}$  
<br>

따라서, 총 시간 복잡도는 $\mathrm{O(E \ logV + V \ logV)} = \mathrm{O((E + V) \ logV)}$  
그런데, 보통 $E > V$ 이므로 $\mathrm{O(E \ logV)}$ 로 표현하기도 한다.
