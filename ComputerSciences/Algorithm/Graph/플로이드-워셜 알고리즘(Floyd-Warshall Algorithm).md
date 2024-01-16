## Floyd-Warshall Algorithm
**`#그래프`** **`#최단 경로`** **`#모든 노드 간의 최단 경로`** **`#음수 간선 사용 가능`**  

- 모든 노드 간의 최단 경로를 구할 수 있는 알고리즘
- 음의 간선이 존재해도 사용 가능
- 2차원 인접 행렬을 구성하여 DP 방식으로 해답을 구해나가는 것이 특징
- 임의의 노드 **`s`** 에서 **`e`** 까지 가는 데 걸리는 최단거리를 구하기 위해, s와 e 사이의 노드인 **`m`** 에 대해 **`s 👉🏻 m 최단거리`** 와 **`m 👉🏻 e 최단거리`** 를 이용 

<br>

## 알고리즘 동작 과정

- 노드의 개수 `N`에 대해, 3중 반복문을 통해 쉽게 구현 가능
- `s👉🏻e 거리`와 `s👉🏻m + m👉🏻e` 거리 중 최소값을 택하는 방식

$$ D_{se} = min(D_{se}, \ D_{sm} + D_{me})$$  
<br>

- `행`을 `출발 노드 번호`, `열`을 `도착 노드 번호`라 하고, 그 `요소`는 `비용`이라고 할 때, 다음과 같은 2차원 인접 행렬을 생성할 수 있다. (아래는 예시)

| 출발 \ 도착 | 1   | 2   | 3   | 4   |
| ----------- | --- | --- | --- | --- |
| 1           | 0    | 4    | INF    | 6    |
| 2           | 3    | 0    | 7    | INF    |
| 3           | 5    | INF    | 0    | 4    |
| 4            | INF    | INF    | 2    | 0    |

- 이렇게 생성한 2차원 인접 배열을 통해, 다음과 같이 플로이드 워셜 알고리즘을 적용할 수 있다.
```cpp
void FloydWarshall(vector<vector<int>>& arr, int N)
{
	for (int mid = 1; mid <= N; mid++)
		for (int from = 1; from <= N; from++)
			for (int to = 1; to <= N; to++)
				arr[from][to] = min(arr[from][to], arr[from][mid] + arr[mid][to]);
}
```  

## 예제 문제
[🔗Programmers Lv3. 합승 택시 요금](https://school.programmers.co.kr/learn/courses/30/lessons/72413)  

```cpp
#include <vector>

using namespace std;
using uint32 = unsigned int;
#define INF 3000000000

void FloydWarshall(vector<vector<uint32>>& graph, int n)
{
    for (int mid = 1; mid <= n; mid++)
        for (int from = 1; from <= n; from++)
            for (int to = 1; to <= n; to++)
                graph[from][to] = min(graph[from][to], graph[from][mid] + graph[mid][to]);
}

int solution(int n, int s, int a, int b, vector<vector<int>> fares) {
    // 1. Input Initialize
    vector<vector<uint32>> graph(n + 1, vector<uint32>(n + 1, INF));
    for (int i = 1; i <= n; i++)
        graph[i][i] = 0;

    for (const vector<int>& fare : fares)
    {
        uint32 from = fare[0], to = fare[1], cost = fare[2];
        graph[from][to] = graph[to][from] = cost;
    }

    // 2. Floyd-Warshall Algorithm
    FloydWarshall(graph, n);
    uint32 answer = INF;

    for (int i = 1; i <= n; i++)
        answer = min(answer, graph[s][i] + graph[i][a] + graph[i][b]);
    return answer;
}
```  

<br>

## 성능 측정

- 노드의 개수를 `N`이라 했을 때, 3중 반복문을 돌리므로 시간 복잡도는 $\mathrm{O(N^3)}$


