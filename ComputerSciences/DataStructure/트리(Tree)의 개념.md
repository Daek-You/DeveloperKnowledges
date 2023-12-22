## 1. 트리(Tree)

![트리](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Binary_tree.svg/300px-Binary_tree.svg.png)  
출처 : [Wikipedia](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EA%B5%AC%EC%A1%B0)  

- **`루트(Root)`** 노드를 시작으로, 마치 나무 가지가 뻗어 나가는 듯한 모습을 가진 자료구조
- 회사 조직도, 의사 결정 나무, 게임의 스킬 트리, 운영체제의 파일 시스템 등 다양한 곳에서 응용  
- 이진 트리, 이진 탐색 트리, 균형 트리 등등 다양한 종류가 존재  
<br>

## 2. 트리의 용어들  

![트리의 구조](https://i.namu.wiki/i/le4r6gZhWmQIcZy0dh73JA1DeEx68JfCDHZS-P47eoRSNSOklgFXWwj6e3yLCNHyksT5gVwE4cI1zqsDC5zdQnoavf20U7yoDSaQvNqeOmtag8ENOnP34XPlGnwGHGIl4gYffb1vRH6SeeSLDUG6fg.webp)    
(출처 : [나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)))   

| 용어               | 의미                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| **`Root Node`**    | 트리의 최상단에 존재하는 노드                                                                         |
| **`Branch Node`**  | 루트 노드와 리프 노드 사이에 있는 노드들                                                              |
| **`Leaf Node`**    | **`단말 노드(Terminal Node)`** 라고도 부르며, Branch Node 끝에 달린 노드                              |
| **`Parent Node`**  | 자신보다 위에 있는 연결된 노드이며, 루트 노드는 부모 노드가 없음                                      |
| **`Child Node`**   | 자신보다 아래에 있는 연결된 노드                                                                      |
| **`Sibling Node`** | 같은 부모에서 파생된 같은 레벨의 다른 노드                                                            |
| **`Path`**         | 한 노드에서 다른 노드까지의 길 사이에 있는 노드들의 순서 (ex. A $\rightarrow$ D 의 경로는 [A, B, D] ) |
| **`Length`**       | 출발 노드에서 목적지 노드까지 거쳐야 하는 노드의 개수 (ex. A $\rightarrow$ D 의 길이는 2)             |
| **`Depth`**        | 루트 노드에서 해당 노드까지 이르는 경로의 길이 (ex. A $\rightarrow$ F 의 깊이는 2)                    |
| **`Level`**        | 깊이가 같은 노드들의 집합 (ex. 레벨 2인 노드들은 D, E, F, G)                                          |
| **`Height`**       | 가장 깊은 곳에 있는 리프 노드까지의 깊이 (ex. H이므로 트리의 높이는 3)                                |
| **`Degree`**       | 해당 노드의 자식 노드 개수 (트리의 차수는 자식 노드가 가장 많은 노드의 차수)                          |
| **`Sub Tree`**           | 트리 내에 존재하는 부분 집합 트리                                                                     |  

<br>

## 3. 트리의 표현법

### $N$ - 링크

![N-Link](https://images.velog.io/images/kdkeiie8/post/4a32713c-7a2f-4812-95b2-509d73a717ab/IMG_D75D60566E33-1.jpeg)  
출처 : [개발자로 전직합니다(Velog)](https://velog.io/@kdkeiie8/%ED%8A%B8%EB%A6%AC-%ED%8A%B8%EB%A6%AC%EC%99%80-%EB%85%B8%EB%93%9C%EC%9D%98-%ED%91%9C%ED%98%84-%EB%B0%A9%EB%B2%95)  


- 노드의 차수가 $N$ 이라면, 각 노드는 $N$ 개의 링크를 가지고 있음
- 각 노드마다 차수가 다르다면, 일반적으로 적용하기 어려운 방법
- 가변 배열이나 연결 리스트 형태로 구현할 수 있겠지만, 복잡해지는 문제가 존재
- 이를 해결하기 좋은 방법은 **`왼쪽 자식-오른쪽 형제(Left Child-Right Sibling)`** 표현법  
<br>

### 왼쪽 자식-오른쪽 형제(Left Child-Right Sibling)

![LCRS](https://wiselotis.github.io/assets/posts_con/tree/lcrs.jpg)   
출처 : [hackmd.io](https://hackmd.io/@ZFTA55hvQPaj10x_apz7hA/HyRKikzaL)  

- 왼쪽 링크에는 자식 노드를, 오른쪽 링크에는 형제 노드를 가리켜 표현하는 방법
- 이 방법으로 모든 트리를 [**이진 트리(Binary Tree)**](이진%20트리(Binary%20Tree).md) 형태로 재구성이 가능  
