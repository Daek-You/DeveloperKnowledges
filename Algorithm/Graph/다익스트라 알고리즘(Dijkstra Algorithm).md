## 다익스트라 알고리즘(Dijkstra Algorithm)

**`#그래프`** **`#최단 경로`** **`#Greedy`** **`#Dynamic Programming`**  **`#한 노드에서 다른 모든 노드와의 최단 경로`**  

- 다익스트라가 고안한 최단 경로 탐색 알고리즘
- **`음의 간선` 이 없는 경우에 사용 가능**
<br>

### 알고리즘 동작 과정
1. **한 정점에서부터 각 정점으로까지의 최단 거리를 저장하는 배열 생성**  
	맨 처음에는 시작점을 제외한 최단 경로를 알지 못하므로 시작점은 0, 나머지는 무한대($\infty$) 로 초기화
<br>

2. **방문하지 않은 노드들 중에서 거리가 가장 짧은 노드 선택**  
	여기에서 **`우선순위 큐(Priority Queue)`** 를 활용
<br>

3. **선택한 노드를 거쳐 인접한 노드로 가는 비용 계산**  
	기존의 최소 비용보다 저렴하다면, 갱신
<br>

최단 거리는 `여러 개의 최단 거리`로 이루어져 있기 때문에, 다이나믹 프로그래밍을 기반으로 사용할 수 있다. 알고리즘 동작 과정에 대한 시뮬레이션은 아래 블로그에서 잘 정리해두었으므로, 들어가서 보는 것을 추천한다.  
- 🔗[[알고리즘] 다익스트라 알고리즘](https://velog.io/@717lumos/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BCDijkstra-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
- 🔗[다익스트라 알고리즘](https://m.blog.naver.com/ndb796/221234424646)
- 🔗[핵심 자료구조 - 그래프 : 최단 경로 : BFS, 다익스트라](https://velog.io/@kasterra/%ED%95%B5%EC%8B%AC-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%9E%98%ED%94%84-%EC%B5%9C%EB%8B%A8-%EA%B2%BD%EB%A1%9C-%ED%83%90%EC%83%89#%EA%B0%80%EC%A4%91%EC%B9%98%EA%B0%80-%EB%8B%A4%EB%A5%B4%EA%B3%A0-%EC%9D%8C%EC%88%98%EA%B0%84%EC%84%A0%EC%9D%B4-%EC%97%86%EC%9D%84%EB%95%90-%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC)
<br>

### 소스 코드
🔗[BOJ_1753 - 최단경로](https://www.acmicpc.net/problem/1753)

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;
#define FAST_IO ios::sync_with_stdio(false); std::cout.tie(NULL); std::cin.tie(NULL);
#define INF 1000000000

void Dijkstra(vector<vector<pair<int, int>>>& adjacentNodes, vector<int>& distances, int start, int V, int E) {

    distances[start] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.emplace(0, start);

    while (!pq.empty()) {
        int cost = pq.top().first;
        int node = pq.top().second;
        pq.pop();

        if (distances[node] < cost)     // 최단 거리가 아닌 경우, Skip
            continue;
        
        for (const auto& adjacentNode : adjacentNodes[node]) {
            int next = adjacentNode.first;
            int newDistance = cost + adjacentNode.second;
            
            // 기존 거리 vs 인접 노드를 거쳐 간 새로운 거리 중 작은 걸 선택
            if (distances[next] > newDistance) {
                distances[next] = newDistance;
                pq.emplace(newDistance, next);
            }
        }
    }
}

int main() {
    FAST_IO
    int V, E, startNode;
    cin >> V >> E >> startNode;

    vector<vector<pair<int, int>>> adjacentNodes(V + 1, vector<pair<int, int>>());  // i번째로부터의 [<연결된 정점 번호, 가중치>]
    int from, to, cost;
    for (int i = 0; i < E; i++) {
        cin >> from >> to >> cost;
        adjacentNodes[from].emplace_back(to, cost);
    }

    vector<int> distances(V + 1, INF);                        // 최소 비용
    Dijkstra(adjacentNodes, distances, startNode, V, E);

    for (int i = 1; i <= V; i++) {
        if (distances[i] == INF)
            cout << "INF" << "\n";
        else
            cout << distances[i] << "\n";
    }
    
    return 0;
}
```
<br>

## 성능 측정

$$ \mathrm{O((V+E) \cdot log(N))}$$
<br>

> $\mathrm{V}$ : `정점(Vertex)`의 개수,  $\mathrm{E}$ : `간선(Edge)`의 개수

- 우선순위 큐 계산은 $\mathrm{O(log(N))}$, 이를 최대 $V$ 번 진행하므로 $\mathrm{O(V \cdot log(V))}$
- 최대 모든 노드 및 간선에 대해 우선순위 큐 계산을 해줘야 하므로 $(V+E) \cdot log(N)$
