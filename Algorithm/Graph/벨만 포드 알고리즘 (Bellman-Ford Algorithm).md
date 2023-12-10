#그래프 #최단경로 #One-To-Many #완전탐색 

### 소개
- [[다익스트라 알고리즘(Dijkstra Algorithm)]]과 마찬가지로, 한 정점 👉🏻 다른 모든 정점으로 가는 최단 경로를 구하는 알고리즘
- 다익스트라 알고리즘보단 느리지만, `음수 간선`이 있는 경우에도 사용 가능
-  `음수 간선의 사이클`이 있는 경우에도 감지 가능

### 알고리즘 동작 과정
<u>매 단계마다 모든 노드를 방문하여, 모든 노드 간의 최단 거리를 구하는 것</u>이 핵심
`총 노드의 개수` : $V$,  `총 간선의 개수` : $E$
<br>

1. **최단거리 테이블을 생성하고, 시작점을 0, 다른 노드들은 무한대($\infty$)로 초기화**
<br>
2. **$(V - 1)$ 번 만큼 사이클을 돌며, 다음 과정을 진행한다.**
	1) 모든 간선 $E$ 개를 하나씩 확인한다.
	2) 각 간선을 거쳐, 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신한다.
<br>
3. **만약 음수 간선의 순환이 발생하는지 체크하고 싶다면, 2번 과정을 한 번 더 수행한다.**
	- 수행 후, 최단 거리 테이블이 갱신된다면 음수 간선 순환이 존재하는 것

### 소스 코드
🔗[BOJ_11657 - 타임머신](https://www.acmicpc.net/problem/11657)

```cpp
#include <iostream>
#include <vector>

using namespace std;
#define FAST_IO ios::sync_with_stdio(false); cout.tie(NULL); cin.tie(NULL);
#define INF 1000000000

struct Edge {
    Edge() { }
    Edge(int from, int to, long long cost) : from(from), to(to), cost(cost) { }
    int from, to;
    long long cost;
};

bool BellmanFord(const vector<Edge>& busSchedules, vector<long long>& times, int nodeCount, int edgeCount, int start) {
    times[start] = 0;

    for (int i = 0; i < nodeCount; i++) {
        for (const auto& schedule : busSchedules) {
            int from = schedule.from;
            int to = schedule.to;
            long long cost = schedule.cost;
            long long newCost = times[from] + cost;

            if (times[from] != INF and newCost < times[to])
                times[to] = newCost;
        }
    }

    // 음의 사이클 점검
    for (const auto& schedule : busSchedules) {
        int from = schedule.from;
        int to = schedule.to;
        long long cost = schedule.cost;
        long long newCost = times[from] + cost;

        if (times[to] != INF and newCost < times[to])
            return true;
    }

    return false;
}

void Print(bool hasNegativeCycle, const vector<long long>& times, int cityCount) {
    if (hasNegativeCycle) {
        cout << "-1";
        return;
    }

    for (int i = 2; i <= cityCount; i++) {
        if (times[i] == INF)
            cout << "-1" << "\n";
        else
            cout << times[i] << "\n";
    }
}

int main() {
    FAST_IO
    int cityCount, busRouteCount;
    cin >> cityCount >> busRouteCount;

    vector<Edge> busSchedules;
    int from, to, cost;

    for (int i = 0; i < busRouteCount; i++) {
        cin >> from >> to >> cost;
        busSchedules.emplace_back(from, to, cost);
    }

    vector<long long> times(cityCount + 1, INF);
    bool hasNegativeCycle = BellmanFord(busSchedules, times, cityCount, busRouteCount, 1);

    Print(hasNegativeCycle, times, cityCount);
    return 0;
}
```


### 성능 측정

$$ \mathrm{O(V \cdot E)} $$

- 노드를 매번 방문할 때마다 모든 간선을 살펴보므로, $V \times E$
