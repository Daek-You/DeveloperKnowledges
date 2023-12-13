
## 1. 이진 트리(Binary Tree)란?

![이진 트리](https://i.namu.wiki/i/59LWn3f8_eNGViAn3B4Fn7PDpL802gevs7nPz-hAd3epT2eKPI7bFA7ZyWgvs__ggzq4BJFEm-fyRELROJFf_A5VSdJks7RwrRSWq7gr-r6ROZkz8FF0kkekmAlOYGGS4ZEKYh9MYdWn4rk_ieN-AA.gif)  
출처 : [나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84))  

```cpp
struct Node {
	int data;
	Node* left, right;
};
```  

- **`차수(자식 노드의 개수)`** 가 **최대 2인 트리**로, **`왼쪽 자식`** 과 **`오른쪽 자식`** 으로 구분
- 다항식의 계산, 탁월한 검색 성능의 이진 탐색 트리와 같은 대표적인 알고리즘의 기반
<br>

## 2. 이진 트리의 종류

### 1) 포화 이진 트리(Full Binary Tree)

![포화 이진 트리](https://web.cecs.pdx.edu/~sheard/course/Cs163/Graphics/FullBinary.jpg)  
출처 : [web.cecs.pdx.edu](https://web.cecs.pdx.edu/~sheard/course/Cs163/Doc/FullvsComplete.html)  

- **리프 노드들을 제외한 모든 노드들이 자식을 2개씩 가지고 있는 이진 트리**
- 리프 노드들이 모두 같은 깊이에 위치하는 것이 특징
- 트리의 높이가 $N$ 이라면, 해당 포화 이진 트리에서 노드의 최대 개수는 $2^N - 1$ 개
<br>

### 2) 완전 이진 트리(Complete Binary Tree)

![완전 이진 트리](https://scaler.com/topics/images/introduction-to-complete-binary-tree.webp)  
출처 : [scaler](https://www.scaler.com/topics/complete-binary-tree/)  

- **리프 노드들이 왼쪽부터 차곡차곡 쌓인 트리**
- 포화 이진 트리의 전 단계로, 포화 이진 트리의 부분 집합이라고도 볼 수 있음  

<br>

## 3. 이진 트리의 순회(Traversal)

### 1) 전위 순회(Pre-order Traversal)

- **`자기 자신 👉🏻 왼쪽 자식 👉🏻 오른쪽 자식` 순으로 방문하는 방법**
- 이진 트리를 중첩된 괄호로 표현이 가능  

```cpp
void PreOrder(Node* currentNode) {
	if (currentNode == nullptr)
		return;
	
	std::cout << currentNode->data << ", ";
	PreOrder(currentNode->left);
	PreOrder(currentNode->right);
}
```  

### 2) 중위 순회(In-order Traversal)

- **`왼쪽 자식 👉🏻 자기 자신 👉🏻 오른쪽 자식`** 순으로 방문하는 방법
- [**이진 탐색 트리**](이진%20탐색%20트리(Binary%20Search%20Tree).md)에서는 **오름차순** 순서로 방문하는 데에 활용 가능
- [**수식 트리**](수식%20트리(Expression%20Tree).md)에서 **`중위 표기식`** 을 나타내는 데에도 활용  

```cpp
void InOrder(Node* currentNode) {
	if (currentNode == nullptr)
		return;

	Inorder(currentNode->left);
	std::cout << currentNode->data << ", ";
	Inorder(currentNode->right);
}
```  

### 3) 후위 순회(Post-order Traversal)

- **`왼쪽 자식 👉🏻 오른쪽 자식 👉🏻 자기 자신`** 순으로 방문하는 방법
- [**수식 트리**](수식%20트리(Expression%20Tree).md)에서 **`후위 표기식`** 을 나타내는 데에 활용
- 트리를 파괴할 때 리프 노드부터 파괴해야 하는데, 이 때에도 후위 순회 활용 가능  

```cpp
void PostOrder(Node* currentNode) {
	if (currentNode == nullptr)
		return;

	PostOrder(currentNode->left);
	PostOrder(currentNode->right);
	std::cout << currentNode->data << ", ";
}
```  


