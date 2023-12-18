## 1. 위상 정렬(Topological Sort) 이란?

- **`순서가 정해져 있는 작업`** 을 차례대로 수행해야 할 때, 그 순서를 결정하기 위해 사용
- 대표적인 예시로, 대학교의 **선수과목**을 들 수 있음  

![영남대학교 컴퓨터공학과 선수과목](https://www.yu.ac.kr/_res/yu/cse/img/abeek/img-readmap01.gif)   

[출처 : 영남대학교 컴퓨터공학과 선수과목 로드맵]  

- 선수과목 로드맵을 통해, 1학년부터 4학년까지 들을 과목들을 일렬로 나열할 수 있음
- 이렇게 일렬로 나열하여 시작점부터 목적지까지 순서대로 나열한 것을 **`위상 정렬`** 이라고 함
- **`사이클이 없는 방향성 그래프(DAG, Directed Acyclic Graph)`** 에서만 위상 정렬 사용 가능
- 사이클이 존재한다면, 시작점을 찾을 수 없기에 사용할 수 없음

<br>

## 2. 알고리즘 동작 과정  

### Kahn 알고리즘

**`큐(Queue)`** 자료구조를 활용하는 방식으로, 동작 과정은 다음과 같다.  

1. **`진입차수(In-Degree)` 가 0 인 노드를 전부 큐에 집어 넣는다.**
2. **큐에서 노드를 하나 뽑아, 해당 노드가 가리키는 모든 노드의 진입차수를 1 씩 줄인다.**
3. **큐가 빌 때까지, 2번 과정을 반복한다.**
	- 모든 원소를 방문하기 전에 큐가 빈다면, **사이클이 존재하는 것**  
4. **그래프가 완전히 비지 않았다면, 1번 과정으로 되돌아 간다.**  
	- 모든 원소를 방문했다면, **`큐에서 꺼낸 순서`** 가 **위상 정렬의 결과**    

<br>

### 소스 코드

[🔗[BOJ_2252] 줄 세우기](https://www.acmicpc.net/problem/2252) 문제를 예제로 선택  

```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;
#define FAST_IO ios::sync_with_stdio(false); cout.tie(NULL); cin.tie(NULL);

void TopologicalSort(const vector<vector<int>>& graph, vector<int>& answers, vector<int>& inDegrees, int N) {

    queue<int> students;
    for (int i = 1; i <= N; i++)
        if (inDegrees[i] == 0)
            students.push(i);

    for (int i = 1; i <= N; i++) {
        if (students.empty())   // 사이클 발생
            return;

        int node = students.front();
        answers[i] = node;
        students.pop();

        for (const int& otherNode : graph[node]) {
            if (--inDegrees[otherNode] == 0)
                students.push(otherNode);
        }
    }
}
```  

```cpp
int main() {

    FAST_IO
    int N, M;
    cin >> N >> M;

    int front, back;
    vector<vector<int>> graph(N + 1, vector<int>());
    vector<int> inDegrees(N + 1, 0);

    for (int i = 0; i < M; i++) {
        cin >> front >> back;
        graph[front].emplace_back(back);
        inDegrees[back]++;
    }

    vector<int> answers(N + 1);
    TopologicalSort(graph, answers, inDegrees, N);

    for (int i = 1; i <= N; i++)
        cout << answers[i] << " ";

    return 0;
}
```  

<br>

## 성능 측정

$$ \mathrm{O(V + E)}$$  
- 정점의 개수(V) + 간선의 개수(E) 만큼 시간이 소요됨  
