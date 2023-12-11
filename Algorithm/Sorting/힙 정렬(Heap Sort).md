## 1. 힙 정렬(Heap Sort)이란?
- [**힙(Heap)**](../../DataStructure/힙(Heap).md) 자료구조를 기반으로 한 정렬 방식  
- [**병합 정렬**](병합%20정렬(Merge%20Sort).md)과 다르게, 추가적인 메모리를 요구하지 않음
- 항상 $\mathrm{O(N \times logN)}$ 의 일정한 성능을 보여줌  
<br>

## 2. 힙 정렬의 알고리즘
- **루트 노드에서 항상 최댓값 또는 최솟값이 나오는 것에 기반**함  
- 힙 자료구조에 기반하여 정렬이 이루어지기 때문에, **`heapify`** 라는 연산이 필요  
> **`heapify`** : 주어진 자료구조가 힙의 성질을 만족하도록 하는 연산  

### 힙 정렬 순서 (오름차순)

1. **주어진 배열을 통해, 최대 힙(Max heap)으로 구조를 변경**  👉🏻  **`heapify`** 연산  
2. **`맨 뒷 값(최솟값)`** 과 `루트 값(최댓값)` 을 교환하고, 루트부터 힙 구조가 깨지지 않았는지 **`heapify`** 연산에 들어간다.
3. 힙을 다 돌 때까지, 크기를 하나씩 줄여가며 2번을 진행한다.  
<br>

## 3. C++로 구현한 힙 정렬
```cpp
#include <vector>
using namespace std;

void Swap(vector<int>& arr, int i, int j)
{
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}

void Heapify(vector<int>& arr, int size, int parent)
{
	int child = parent * 2;

	if (child < size - 1 and arr[child] < arr[child + 1])
		child++;

	if (child < size and arr[parent] < arr[child])
		Swap(arr, parent, child);

	if (child < size / 2)
		Heapify(arr, size, child);
}

void HeapSort(vector<int>& arr)
{
	int N = arr.size();

	// 1. 최대 힙 구성
	for (int current = N / 2; current >= 0; current--)
		Heapify(arr, N, current);

	// 2. 최솟값을 맨 앞에 차례 차례 옮기고, 힙이 깨지진 않는지 재구성
	for (int i = N - 1; i >= 0; i--)
	{
		Swap(arr, i, 0);
		Heapify(arr, i, 0);
	}
}
```  
<br>

## 4. 힙 정렬의 성능 분석
### 장점
- **`최댓값`** 이나 **`최솟값`** 을 구할 때 유용
- 항상 $\mathrm{O(N \times logN)}$ 의 일정한 성능을 보여줌  

### 단점
- **`안정성(Stable)`** 을 보장하지 않는다.
- 데이터의 상태에 따라, 시간이 더 오래 걸릴 수 있다.  

### 시간 복잡도
> **최선, 평균, 최악**  
- $\mathrm{O(N \times logN)}$  
