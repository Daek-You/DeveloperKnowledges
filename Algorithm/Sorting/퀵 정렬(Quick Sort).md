# 퀵 정렬(Quick Sort)  

> **목차**  
> 1. [퀵 정렬이란?](#1-퀵-정렬quick-sort이란)  
> 2. [퀵 정렬의 알고리즘](#2-퀵-정렬의-알고리즘)  
> 3. [퀵 정렬 알고리즘 구현에 대한 고민](#3-퀵-정렬-알고리즘-구현에-대한-고민)  
> 4. [C++로 구현한 퀵 정렬](#4-c로-구현한-퀵-정렬)  
> 5. [퀵 정렬의 성능 분석](#5-퀵-정렬의-성능-분석)  

<br>

## 1. 퀵 정렬(Quick Sort)이란?
- `분할 정복(Divide and Conquer)`를 기반으로 둔 알고리즘
- `기준 요소(Pivot)`을 선정하고, 해당 `pivot`을 기준으로 분할하는 것이 특징
- 리스트를 균등하게 분할하는 [병합 정렬(Merge Sort)](병합%20정렬(Merge%20Sort).md)과 달리, 퀵 정렬은 비균등하게 분할  

<br>

## 2. 퀵 정렬의 알고리즘
1. `기준 요소(Pivot)`을 선정하고, 정렬 대상을 분류  

    > - 자료구조에서 `Pivot`의 값을 임의로 선정  
    > - `요소 < Pivot`인 요소는 <u>Pivot의 왼쪽</u>으로 이동  
    > - `요소 > Pivot`인 요소는 <u>Pivot의 오른쪽</u>으로 이동  

<br>

2. 정렬 대상을 분할하기  

    > - 1번 과정에 의해, Pivot의 왼쪽에는 Pivot보다 작은 요소들의 그룹이 생김  
    > - 또한, Pivot의 오른쪽에는 Pivot보다 큰 요소들의 그룹이 생김  
    > 여기에서 왼쪽 그룹과 오른쪽 그룹을 분할하여, 각 그룹에 대해 1번 과정을 다시 수행  

<br>

3. `그룹의 크기`가 `1 이하`여서 더 이상 분할할 수 없을 때까지 1, 2번 과정 반복  

<br>

## 3. 퀵 정렬 알고리즘 구현에 대한 고민
1. 배열을 사용할 경우, 퀵 정렬의 분할 과정을 어떻게 효율적으로 처리할 것인가?  
    👉🏻 왼쪽과 오른쪽에서 서로를 향해 이동하는 `index` 변수를 통해 해결이 가능  
    
    > - 왼쪽 끝에서 시작하는 변수 `left`는 오른쪽 방향으로 이동하면서 `Pivot`보다 큰 요소를 찾음  
    > - 오른쪽 끝에서 시작하는 변수 `right`는 왼쪽 방향으로 이동하면서 `Pivot`보다 작은 요소를 찾음  
    > - `left`, `right` 둘 다 조건에 맞는 요소를 찾았다면, 서로 요소를 교환  
    > - `left > right`가 될 때까지 위 과정을 반복 후, `Pivot`을 `왼쪽 그룹의 마지막 요소(right)`와 교환  

<br>

> 1번 과정 시뮬레이션  

```cpp
/* p: pivot, l: left, r: right */

// p   l           r      맞교환  p   l           r
   5 1 6 4 8 3 7 9 2       ->     5 1 2 4 8 3 7 9 6

// p       l r            맞교환  p       l r
   5 1 2 4 8 3 7 9 6       ->     5 1 2 4 3 8 7 9 6

// 아래의 경우, left > right가 되었으므로 pivot을 r과 교환
// p       r l                    r       p l
   5 1 2 4 3 8 7 9 6       ->     3 1 2 4 5 8 7 9 6

// 이렇게 함으로써, p = 5보다 작은 그룹은 왼쪽에, 큰 그룹은 오른쪽에 위치하게됨
// 이렇게 나뉘어진 왼쪽, 오른쪽 그룹에 대해 다시 1번 과정을 진행 -> 그룹의 크기가 1이 될 때까지 진행
``` 

2. 반복되는 분할 과정은 어떻게 처리한 것인가?  👉🏻  `재귀(Recursion)`를 통해 해결  

<br>

## 4. C++로 구현한 퀵 정렬
`제일 왼쪽 요소`를 `Pivot`으로 선택하는 방법을 사용하였습니다.
```cpp
#include <vector>
using namespace std;

void Swap(int& x, int& y)
{
	int temp = x;
	x = y;
	y = temp;
}

int Partition(vector<int>& arr, int left, int right)
{
	int first = left;
	int pivot = arr[first];
	++left;
	
	while (left <= right)
	{
		// pivot보다 큰 요소 찾기
		while (arr[left] <= pivot and left < right)
			++left;

		// pivot보다 작은 요소 찾기
		// left < right로 조건을 설정하면, 위 while 문에서 left == right이 되었을 경우, 아래 while문이 동작하지 않는다.
		while (arr[right] >= pivot and left <= right)  
			--right;

		if (left >= right)
			break;
		Swap(arr[left], arr[right]);
	}

	Swap(arr[first], arr[right]);  // pivot과 왼쪽 그룹 제일 마지막 요소 교환
	return right;                  // pivot 위치 반환
}

void QuickSort(vector<int>& arr, int left, int right)
{
	if (left >= right)
		return;
	
	int mid = Partition(arr, left, right);

	QuickSort(arr, left, mid - 1);   // 왼쪽 그룹 분할
	QuickSort(arr, mid + 1, right);  // 오른쪽 그룹 분할
}
```  

<br>

## 5. 퀵 정렬의 성능 분석
### 장점
- 평균 시간 복잡도가 $\mathrm{N \times log(N)}$이며, 다른 $\mathrm{N \times log(N)}$ 알고리즘에 비해 대체적으로 속도가 매우 빠름
- `제자리 정렬(in-place-sorting)` 방식 👉🏻 물론 재귀 호출을 위한 스택 공간 복잡도가 $\mathrm{logN}$이지만, 이 마저도 매우 적은 수준  

### 단점
- 재귀를 사용하지 못하는 환경일 경우, 구현이 매우 복잡해진다.
- `Pivot` 선정에 따라 성능이 차이날 수 있다.
- `안정성(Stable)`을 보장하지 않는다.
- 정렬이 된 상태의 자료구조에 적용할 경우, 최악의 성능을 보여준다.  👉🏻  $\mathrm{O(N^2)}$  

    > - 예를 들어, 오름차순 정렬된 배열에 가장 왼쪽 요소를 Pivot으로 잡는다면?  
    > - 왼쪽의 부분 리스트는 없고, 오른쪽에 N-1개의 요소를 갖는 부분 리스트가 생성됨  
    > - 이 경우, 재귀 레벨이 $\mathrm{logN}$이 아니라, N까지 내려가게 됨  👉🏻 $\mathrm{O(N^2)}$  
    > - 가장 오른쪽 요소를 Pivot으로 잡아도, 왼쪽으로 치중되기에 문제가 발생하는 건 마찬가지  
    > - 위처럼 한 쪽으로 치중될 경우에는 공간 복잡도 또한 $\mathrm{N}$을 차지할 수 있음  
    > - 이러한 단점 때문에, `중간 피벗 방법`이 선호됨  

### 시간 복잡도
> 최선, 평균  
- 데이터가 무작위로 배치되어 있는 경우  👉🏻  $\mathrm{N \times log(N)}$  

> 최악
- 데이터가 정렬되어 있는 경우  👉🏻  $\mathrm{O(N^2)}$  











