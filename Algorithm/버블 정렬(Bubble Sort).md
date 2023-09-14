## 1. 버블 정렬(Bubble Sort)이란?
- <u>서로 인접한 두 원소를 검사하여 정렬</u>하는 간단한 알고리즘
- 기본 개념이 선택 정렬(Selection Sort)과 유사
- 가장 간단한 방법이지만, 그만큼 성능이 좋지 않아 쓰이지 않는 알고리즘  

<br>

## 2. 버블 정렬의 알고리즘
- 크기가 $n$인 배열 `arr`가 있고, 정렬 기준이 **오름차순**이라면, 버블 정렬은 다음과 같은 과정을 거치게 된다.  

	> 1. `arr[0]`와 `arr[1]`을 비교 → `arr[0] > arr[1]`이라면, 서로 값을 교환
	> 2. `arr[1]`과 `arr[2]`를 비교 → `arr[1] > arr[2]`이라면, 서로 값을 교환
	> 3. ...
	> 4. `arr[n-2]`와 `arr[n-1]`을 비교 → `arr[n-2] > arr[n-1]`이라면, 서로 값을 교환  
	> 👉🏻 위와 같이 1 회전을 했다면, `arr[n-1]`에 가장 큰 원소가 배치됨  

<br>

- `arr[n-1]`에 올바른 원소가 배치되었으므로, arr[n-1]을 제외한 나머지 원소들로 위 과정을 다시 진행  

	> 1. `arr[0]`와 `arr[1]`을 비교 → `arr[0] > arr[1]`이라면, 서로 값을 교환
	> 2. `arr[1]`과 `arr[2]`를 비교 → `arr[1] > arr[2]`이라면, 서로 값을 교환
	> 3. ...
	> 4. `arr[n-3]`와 `arr[n-2]`을 비교 → `arr[n-3] > arr[n-2]`이라면, 서로 값을 교환  
	> 👉🏻 2 회전을 완료했으므로, `arr[n-2]`에는 두 번째로 큰 원소가 배치됨  

<br>

- 위와 같이 루프를 한 번 돌 때마다 배열의 뒷 쪽 원소를 제외해가며, 총 `n-1`번을 진행하면 정렬이 완료된다.  

#### 버블 정렬 시뮬레이션 (출처: [Tech Interview](https://gyoogle.dev/blog/algorithm/Bubble%20Sort.html))
![버블 정렬 시뮬레이션](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/bubble-sort-001.gif)  

<br>

## 3. C++로 구현한 버블 정렬
```cpp
#include <vector>
using namespace std;

void Swap(vector<int>& arr, int i, int j)
{
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}

void BubbleSort(vector<int>& arr)
{
	int n = arr.size();

	for (int lastIdx = n - 1; lastIdx > 0; lastIdx--)
	{
		bool onSwapEvent = false;

		for (int currentIdx = 0; currentIdx < lastIdx; currentIdx++)
		{
			if (arr[currentIdx + 1] < arr[currentIdx])
			{
				Swap(arr, currentIdx + 1, currentIdx);
				onSwapEvent = true;
			}
		}

		// 원소들이 정렬되어 있다는 의미이므로, 성능 향상을 위해 루프 종료
		if (!onSwapEvent)
			break;
	}
}
```  

<br>

## 4. 버블 정렬의 성능 분석
### 장점
- 구현이 매우 간단하다.
- `제자리 정렬(in-place sorting)`이다.
- `안정성(Stable)`이 보장된다.  

### 단점
- 순서에 맞지 않은 요소를 인접한 요소와 교환한다.
- 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다.
- 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.
- 일반적으로 자료의 교환 작업(SWAP)이 자료의 이동 작업(MOVE)보다 더 복잡하기 때문에 버블 정렬은 단순성에도 불구하고 거의 쓰이지 않는다.  

### 시간 복잡도
- 최선, 평균, 최악 모두 $\mathrm{O(n^2)}$
    - 비교 횟수 : $(n-1)+(n-2)+...+2+1 = \frac{n(n-1)}{2} = \mathrm{O(n^2)} $  

<br>

> **[최선의 상황]** 자료들이 이미 정렬되어 있는 경우
- 자료들의 이동이 발생하지 않음  

<br>

> **[최악의 상황]** 자료들이 역순으로 되어 있는 경우
- 이 경우, 매번 교환 연산이 일어나게 됨
- 한 번 교환하는 데에 3번의 이동(`Swap`)이 필요하므로, 총 $3 \times \frac{n(n-1)}{2}$