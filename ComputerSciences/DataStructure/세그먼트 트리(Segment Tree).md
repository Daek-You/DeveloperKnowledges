[🔗참고1](https://velog.io/@youhyeoneee/%EA%B8%B0%ED%83%80-%EC%9E%90%EB%A3%8C-%EA%B5%AC%EC%A1%B0-Segment-Tree-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8-%ED%8A%B8%EB%A6%AC) [🔗참고2](https://m.blog.naver.com/ndb796/221282210534) [🔗참고3](https://yabmoons.tistory.com/431)  


## 1. 세그먼트 트리(Segment Tree)란?

- 여러 개의 데이터가 연속적으로 존재할 때, **`arr[i] ~ arr[j] 사이의 구간 합`** 을 구하는 일반적인 방법은 하나씩 다 더하는 것
	- 최악의 경우, $\mathrm{O(N)}$ 만큼의 시간이 걸려 데이터의 개수가 많으면 오래 걸릴 수 있음
- 이와 같이 **`특정 구간의 합(최솟값, 최댓값, 곱 등)`** 을 빠르게 구할 수 있는 방법을 모색하여 만들어진 자료구조가 바로 **`세그먼트 트리(Segment Tree)`**
- **이진 트리**의 형태로, $\mathrm{O(logN)}$ 만에 특정 구간의 합을 빠르게 구할 수 있다는 장점이 있음

## 2. 알고리즘

- 데이터의 개수를 $N$ 이라고 했을 때, **`트리의 높이(h)`** 는 $log_2(N)$ **을 반올림 한 값**과 같다.  

	```
	// N = 4
	log2(4) = 2.000 -> ceil(2.000) = 2

	// N = 5
	log2(5) = 2.xxx -> ceil(2.xxx) = 3
	```  

- 세그먼트 트리에 필요한 노드 개수는 $2^{h + 1}$ 보다 작거나 같다.
	- 0번 인덱스는 계산의 편의성을 위해 사용하지 않는다.
		- **`왼쪽 자식 노드의 인덱스`** = 부모 노드의 인덱스 $\times$ 2
		- **`오른쪽 자식 노드의 인덱스`** = 부모 노드의 인덱스 $\times$ 2 + 1

## 3. C++로 구현해 본 세그먼트 트리

```cpp
#include <vector>
#include <cmath>

using namespace std;

template<typename T>
class SegmentTree
{
public:
    SegmentTree(const vector<T>& arr);

    T FindSectionSum(int currentNodeIndex, int startIndex, int lastIndex, int left, int right);
    void UpdateElement(int currentNodeIndex, int targetIndex, int startIndex, int lastIndex, T differenceValue);

private:
    T MakeSegmentTree(const vector<T>& arr, int currentNodeIndex, int startIndex, int lastIndex);

private:
    int _height;
    int _treeSize;
    vector<T> _tree;
};

```  

```cpp
template<typename T>
SegmentTree<T>::SegmentTree(const vector<T>& arr)
{
    int N = arr.size();
    _height = ceil(log2(N));
    _treeSize = 1 << (_height + 1);

    _tree.resize(_treeSize);
    MakeSegmentTree(1, 0, N - 1);
}

template<typename T>
T SegmentTree<T>::MakeSegmentTree(const vector<T>& arr, int currentNodeIndex, int startIndex, int lastIndex)
{

    // 리프 노드까지 내려온 것이므로, 해당 노드 번호에 기존 배열의 원소 집어넣고 리턴
    if (startIndex == lastIndex)
    {
        _tree[currentNodeIndex] = arr[startIndex];
        return _tree[currentNodeIndex];
    }

     int midIndex = (startIndex + lastIndex) / 2;
     int leftChildIndex = currentNodeIndex * 2, rightChildIndex = currentNodeIndex * 2 + 1;

    // 왼쪽, 오른쪽 노드의 합을 구해 부모 노드에 저장하고, 이 과정을 루트 노드까지 반복
     T leftSum = MakeSegmentTree(arr, leftChildIndex, startIndex, midIndex);
     T rightSum = MakeSegmentTree(arr, rightChildIndex, midIndex + 1, lastIndex);
     
    _tree[currentNodeIndex] = leftSum + rightSum;
     return _tree[currentNodeIndex];
}

template<typename T>
T SegmentTree<T>::FindSectionSum(int currentNodeIndex, int startIndex, int lastIndex, int left, int right)
{

    // 1. [left, right]와 [startIndex, lastIndex]가 겹치지 않는 경우 -> 탐색 X
    if (left > lastIndex or right < startIndex)
        return T();

    // 2. [left, right]가 [startIndex, lastIndex]를 완전히 포함하는 경우 -> 탐색 X
    if (left <= startIndex and lastIndex <= right)
        return _tree[currentNodeIndex];

    // 3. [startIndex, lastIndex]가 [left, right]를 완전히 포함하는 경우
    // 4. [left, right]와 [startIndex, lastIndex]가 겹쳐져 있는 경우
    int leftChildIndex = currentNodeIndex * 2, rightChildIndex = currentNodeIndex * 2 + 1;
    int midIndex = (startIndex + lastIndex) / 2;

    T leftSum = FindSectionSum(leftChildIndex, startIndex, midIndex, left, right);
    T rightSum = FindSectionSum(rightChildIndex, midIndex + 1, lastIndex, left, right);
    return leftSum + rightSum;
}

template<typename T>
void SegmentTree<T>::UpdateElement(int currentNodeIndex, int targetIndex, int startIndex, int lastIndex, T differenceValue)
{
    if (targetIndex < startIndex or targetIndex > lastIndex)
        return;

    _tree[currentNodeIndex] += differenceValue;

    if (startIndex == lastIndex)
        return;

    int leftChildIndex = currentNodeIndex * 2, rightChildIndex = currentNodeIndex * 2 + 1;
    int midIndex = (startIndex + lastIndex) / 2;
    UpdateElement(leftChildIndex, targetIndex, startIndex, midIndex, differenceValue);
    UpdateElement(rightChildIndex, targetIndex, midIndex + 1, lastIndex, differenceValue);
}
```  

