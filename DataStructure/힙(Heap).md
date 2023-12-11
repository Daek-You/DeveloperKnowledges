## 1. 힙(Heap) 자료구조란?
- **`완전 이진 트리(Complete Binary Tree)`** 의 일종으로, **`우선순위 큐(Priority Queue)`** 를 위해 만들어진 자료구조
- **`최솟값`** 과 **`최댓값`** 을 쉽게 추출할 수 있도록 설계됨  
- **`중복된 값`** 을 허용 (이진 탐색 트리에서는 허용하지 않음)  
- 탐색을 $\mathrm{O(logN)}$ 만에 할 수 있는 강력함을 자랑  

## 2. 힙의 종류
### 1) 최대 트리와 최대 힙
- **`최대 트리(Max tree)`**
  트리의 모든 노드에 대해, **노드의 데이터 값 $\geq$ 자식 노드의 데이터 값**을 만족하는 트리
- **`최대 힙(Max heap)`**
  최대 트리이면서, 완전 이진 트리    

### 2) 최소 트리와 최소 힙  
- **`최소 트리(Min tree)`**
  트리의 모든 노드에 대해, **노드의 데이터 값 $\leq$ 자식 노드의 데이터 값**을 만족하는 트리
- **`최소 힙(Min heap)`**
  최소 트리이면서, 완전 이진 트리  
<br>

## 3. 우선순위 큐(Priority Queue)란?
- 선입선출(First-In, First-Out) 방식이 아닌, **우선순위의 순서대로 삭제되는 큐**  
> ex) 시뮬레이션 시스템, 비행기 탑승 대기,
> 운영체제의 작업 스케줄링, 네트워크 트래픽 제어, 수치 해석적인 계산 등...  

- 우선순위 큐는 **`배열`** , **`연결리스트`** , **`힙`** 으로 구현이 가능  
- 그 중에서도 **`힙(Heap)`** 으로 구현하는 것이 가장 효율적  
	
	![우선순위 큐를 표현하는 방법](https://gmlwjd9405.github.io/images/data-structure-heap/data-structure-heap-priorityqueue.png)  

<br>

## 4. 힙(Heap) 구현
- 힙은 구현하는 표준적인 자료구조는 **`배열`**  
> - 배열의 인덱스 : **`노드 번호`**  
> - 배열의 원소 : **`데이터`**  

- 구현의 편리함을 위해, **0번 인덱스는 사용하지 않는다.**  
- 특정 위치의 노드 번호는 새로운 노드가 추가되어도 변하지 않는다.  
> ex) 루트 노드의 오른쪽 노드의 번호는 항상 3  

- 배열로 구현하게 되었을 때, 부모 노드와 자식 노드는 다음의 관계를 가진다.  
    > - **`부모 노드`** 의 인덱스 : **`(자식 노드의 인덱스) / 2`**  
    > - **`왼쪽 자식 노드`** 의 인덱스 : **`(부모 노드의 인덱스) * 2`**  
    > - **`오른쪽 자식 노드`** 의 인덱스 : **`(부모 노드의 인덱스) * 2 + 1`**  

<br>

### 노드 추가(Push) 구현

1. 힙에 새로운 요소가 들어오면, **힙의 마지막 노드에 새로운 요소를 삽입**
2. 새로운 노드에서부터 부모 노드를 타고 올라가며, 조건을 검사한다.  
> - **최대 힙에서는 부모 노드가 나보다 큰 지 검사**
>   👉🏻 작다면, Swap하고 부모 노드로 이동하여 다시 검사  
> - **최소 힙에서는 부모 노드가 나보다 작은지 검사**
>   👉🏻 크다면, Swap하고 부모 노드로 이동하여 다시 검사  
> - **만약 계속 타고 올라가다가 루트 노드까지 도달했다면, 루트 노드까지 검사하고 중단**  

<br>

### 노드 삭제(Pop) 구현

- 힙에서 삭제는 항상 **`루트 노드`** 에서만 발생  
- **`(1)루트 노드를 삭제`** 한 후, **`(2)힙 재구성 과정`** 을 진행  
	1. 루트 노드를 삭제 후, **힙의 마지막 노드**를 **루트 노드**로 변경  
	2. 루트 노드부터 아래로 순회하면서, 노드의 값들을 비교하며 힙 재구성  
<br>

## 5. C++로 구현한 힙(Heap)
```cpp
#include <vector>
#include <functional>
using namespace std;


template<typename T>
class Heap
{
public:
	Heap() { _heap.resize(5); }

	void Push(const T& data);
	T Pop();
	void Show();
	bool IsEmpty() { return _heap.empty(); }
	void Swap(T& d1, T& d2);

private:
	vector<T> _heap;
	std::function<bool(const T&, const T&)> _compare = less<T>();
	int _idx = 0;
};

template<typename T>
void Heap<T>::Swap(T& d1, T& d2)
{
	T temp = d1;
	d1 = d2;
	d2 = temp;
}

template<typename T>
void Heap<T>::Push(const T& data)
{
	if (_idx + 1 >= _heap.size())
		_heap.resize(_heap.size() * 2);

	_heap[++_idx] = data;

	for (int i = _idx; i > 1; i /= 2)
	{
		int parentIdx = i / 2;

		if (_compare(data, _heap[parentIdx]))
		{
			Swap(_heap[parentIdx], _heap[_idx]);
			continue;
		}

		break;
	}
}

template<typename T>
T Heap<T>::Pop()
{
	T value = _heap[1];
	_heap[1] = _heap[_idx--];
	
	int parent = 1, child = 2;
	while (child <= _idx)
	{
		if (child < _idx and !_compare(_heap[child], _heap[child + 1]))
			child++;

		if (_compare(_heap[parent], _heap[child]))
			break;

		Swap(_heap[parent], _heap[child]);
		parent = child;
		child *= 2;
	}

	return value;
}

template<typename T>
void Heap<T>::Show()
{
	for (int i = 1; i <= _idx; i++)
		cout << _heap[i] << ", ";
	cout << endl;
}
```  
