## 선형 탐색(Linear Search)

- **`순차 탐색(Sequential Search)`** 라고도 부르며, **처음부터 끝까지 모든 데이터를 차례대로 비교하여 원하는 데이터를 찾는 알고리즘**  👉🏻  $\mathrm{O(N)}$  
- 정렬되지 않은 데이터들 사이에서 원하는 데이터를 찾을 수 있는 유일한 알고리즘
- **`Random Access`** 를 지원하지 않는 [**연결 리스트(Linked List)**](리스트(List).md) 에서도 사용 가능
- 처음부터 끝까지 모든 요소를 검사하기에, 성능이 그리 좋진 않아 데이터 개수가 적은 상황에서 주로 사용  

```cpp
template<typename T>
Node<T>* LinkedList<T>::Search(T data) {
	Node<T>* current = head;
    
    while (current != nullptr) {
        if (current->data == data)
            return current;
        current = current.next;
    }
    
    return nullptr;
}
```  

```cpp
template<typename T>
struct Node {
    T data;
    T* next;
};

template<typename T>
class LinkedList{
public:
	// ...
    Node<T>* Search(T data);

private:
	Node<T>* head;
};
```  
<br>

## 자기 구성 순차 탐색(Self-Organizing Sequential Search)

- 자주 사용되는 항목을 앞 쪽에 배치함으로써, 순차 탐색의 검색 효율을 높이는 방법
- 마트 매장 입구에 진열된 행사 상품, MS 워드의 최근 문서 목록, 배달 어플의 즐겨찾기 등에서 사용 가능
- 자주 사용되는 항목을 선별하는 방법에 따라, 크게 다음과 같은 3가지 방법으로 나뉘어짐
	- **`전진 이동법(Move To Front Method)`**  
	- **`전위법(Transpose Method)`** 
	- **`빈도 계수법(Frequency Count Method)`**   

### 1) 전진 이동법(Move To Front Method)

- **탐색된 항목을 가장 맨 앞으로 옮기는 방법 (연결 리스트에선 헤드 노드로)**
- 한 번 찾은 항목을 다시 찾고자 할 때, 바로 탐색할 수 있는 이점이 존재
- 한 번 탐색된 항목이 다음에 또 다시 검색될 가능성이 높은 데이터에 한해서 좋은 방법  

### 2) 전위법(Transpose Method)

- **탐색된 항목을 바로 이전 항목과 교환하는 방법**
- 해당 항목을 찾을 때마다 점점 앞 쪽으로 이동하게 되는 됨
- 자주 탐색되는 항목들을 빠르게 찾을 수 있도록 성능을 개선시킨 방법  

### 3) 빈도 계수법(Frequency Count Method)

- **데이터 내의 각 요소가 탐색된 횟수를 별도의 공간에 저장해두고, 탐색 빈도 수가 높은 순으로 재배열하는 방법**
- 계수 결과를 저장할 별도의 공간과 재배열에 비용이 소모되기에, 잘 써야 함