# 선택 정렬(Selection Sort)

> **목차**  
> 1. [선택 정렬이란?](#1-선택-정렬selection-sort이란)  
> 2. [선택 정렬의 알고리즘](#2-선택-정렬의-알고리즘)  
> 3. [C++로 구현한 선택 정렬](#3-c로-구현한-선택-정렬)  
> 4. [선택 정렬의 성능 분석](#4-선택-정렬의-성능-분석)  

<br>


## 1. 선택 정렬(Selection Sort)이란?
- **데이터를 넣을 위치를 정하고, 해당 위치에 맞는 데이터를 찾는 알고리즘**
- 앞에서부터 하나씩 올바르게 정렬해 가기에, 다음 자리에서는 그 이전 자리를 고려할 필요가 없음
- 버블 정렬과 유사한 개념이지만, 다음과 같은 차이점이 존재
    - 버블 정렬은 정렬 과정에서 올바른 데이터가 <u>뒤</u>에서부터 쌓임
    - 선택 정렬은 정렬 과정에서 올바른 데이터가 <u>앞</u>에서부터 쌓임  

<br>

## 2. 선택 정렬의 알고리즘
- 크기가 $n$인 배열 `arr`가 있고, 정렬 기준이 오름차순이라고 가정
- `Swap(arr, sourceIdx, targetIdx)` 함수는 `arr[sourceIdx]`와 `arr[targetIdx]`의 값을 서로 교환하는 함수
- 선택 정렬은 다음과 같은 과정을 거치게 된다.

    > 1. `int targetIdx = min(arr[0], min(arr[1], arr[2], ..., arr[n-1]))`의 값을 구해, `Swap(arr, 0, targetIdx)`한다.  
    > 2. `int targetIdx = min(arr[1], min(arr[2], arr[3], ..., arr[n-1]))`의 값을 구해, `Swap(arr, 1, targetIdx)`한다.  
    > 3. ...(계속 진행)  
    > 4. `int targetIdx = min(arr[n-2], arr[n-1])`의 값을 구해, `Swap(arr, n-2, targetIdx)`한다.  
    > 👉🏻 $n-1$번째부턴 비교할 대상이 없기 때문에 $n-2$번째까지만 진행하면 된다.  

<br>

#### 선택 정렬 시뮬레이션 (출처: [Tech Interview](https://gyoogle.dev/blog/algorithm/Selection%20Sort.html))  
![선택 정렬 시뮬레이션](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/selection-sort-001.gif)  

<br>

## 3. C++로 구현한 선택 정렬
```cpp
#include <vector>
using namespace std;

void Swap(vector<int>& arr, int& sourceIdx, int& targetIdx)
{
    int temp = arr[sourceIdx];
    arr[sourceIdx] = arr[targetIdx];
    arr[targetIdx] = temp;
}

void SelectionSort(vector<int>& arr)
{
    int n = arr.size();

    for (int sourceIdx = 0; sourceIdx < n - 1; sourceIdx++)
    {
        int targetIdx = sourceIdx;

        for (int nextIdx = sourceIdx + 1; nextIdx < n; nextIdx++)
        {
            if (arr[nextIdx] < arr[targetIdx])
                targetIdx = nextIdx;
        }

        Swap(arr, sourceIdx, targetIdx);
    }
}
```  

<br>

## 4. 선택 정렬의 성능 분석
### 장점
- 자료의 이동 횟수가 미리 결정되기에, 데이터의 이동이 최소화된다.
- 제자리 정렬(in-place sorting)이다.
- 비교 횟수는 많지만, 버블 정렬에 비해 실제 교환 횟수는 적다. 

### 단점
- 안정성(Stable)이 보장되지 않는다.  
- 선택된 데이터가 자기 자신일지라도, `Swap()`을 진행한다. → 코드 상에서 개선 가능

### 시간 복잡도
- 최선, 최악, 평균 모두 $\mathrm{O(n^2)}$
    - $(n-1)+(n-2)+...+2+1 = \frac{n(n-1)}{2}$ 번의 비교 연산이 수행됨
