# 병합 정렬(Merge Sort)  

> **목차**  
> 1. [병합 정렬이란?](#1-병합-정렬merge-sort이란)  
> 2. [병합 정렬의 알고리즘](#2-병합-정렬의-알고리즘)  
> 3. [C++로 구현한 병합 정렬](#3-c로-구현한-병합-정렬)  
> 4. [병합 정렬의 성능 분석](#4-병합-정렬의-성능-분석)  

<br>

## 1. 병합 정렬(Merge Sort)이란?
- `(1)배열을 더 작은 하위 배열로 나누고` 👉🏻 `(2)각 하위 배열을 정렬`하고 👉🏻 `(3)정렬된 하위 배열을 다시 병합`하여 최종 정렬 배열을 형성하는 방식  

    ![병합 정렬 예](https://media.geeksforgeeks.org/wp-content/uploads/20230706153706/Merge-Sort-Algorithm-(1).png)  

- 더 이상 하위 배열로 나눌 수 없을 때까지 계속해서 배열을 반으로 분할하는 재귀 알고리즘
- 배열에 요소가 하나만 남아, 더 이상 분할할 수 없을 때까지 진행
    - 요소가 하나인 배열은 항상 정렬됨
- 그 후, 정렬된 하위 배열이 하나의 정렬된 배열로 병합됨  👉🏻  이 과정에서 추가적인 메모리 필요

<br>

## 2. 병합 정렬의 알고리즘
1. `분할(Divide)` 단계  

    > 입력 배열을 2개의 크기가 같은 부분 배열로 분할한다.  

2. `정복(Conquer)` 단계  

    > 부분 배열을 정렬한다.  
    > 부분 배열의 크기가 충분히 작지 않으면, 재귀 호출을 이용하여 다시 분할  

3. `결합(Combine)` 단계  

    > 정렬된 부분 배열들을 하나의 배열에 병합한다.  

<br>

> 병합 정렬 시뮬레이션 예  

![시뮬레이션](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Merge-sort-example-300px.gif/220px-Merge-sort-example-300px.gif)  


<br>

## 3. C++로 구현한 병합 정렬
```cpp
#include <vector>
using namespace std;

vector<int> sortedArray;

void Merge(vector<int>& arr, int left, int mid, int right)
{
    int leftIdx = left, rightIdx = mid + 1, arrIdx = left;

    // 1. 나뉘어진 두 그룹의 배열에 대해 작은 값 순서대로 정렬 배열(sortedArray)에 넣기
    while (leftIdx <= mid or rightIdx <= right)
    {
        bool isExistLeft = false, isExistRight = false;
        int leftValue = 0, rightValue = 0;

        if (leftIdx <= mid)
        {
            leftValue = arr[leftIdx];
            isExistLeft = true;
        }

        if (rightIdx <= right)
        {
            rightValue = arr[rightIdx];
            isExistRight = true;
        }
    
        // 왼쪽 값만 남았을 경우, 왼쪽 값으로 나머지 채우기
        if (isExistLeft and !isExistRight)
        {
            sortedArray[arrIdx++] = arr[leftIdx++];
            continue;
        }

        // 오른쪽 값만 남았을 경우, 오른쪽 값으로 나머지 채우기
        if (!isExistLeft and isExistRight)
        {
            sortedArray[arrIdx++] = arr[rightIdx++];
            continue;
        }

        int value = min(arr[leftIdx], arr[rightIdx]);
        value == arr[leftIdx] ? leftIdx++ : rightIdx++;
        sortedArray[arrIdx++] = value;
    }

    // 2. 정렬한 내용을 원래 배열에 다시 삽입
    for (int i = left; i <= right; i++)
        arr[i] = sortedArray[i];

}

void MergeSort(vector<int>& arr, int left, int right)
{
    if (left >= right)
        return;

    int mid = (left + right) / 2;
    MergeSort(arr, left, mid);
    MergeSort(arr, mid + 1, right);
    Merge(arr, left, mid, right);
}
```  
![결과](../Resources/Images/병합%20정렬%20적용%20예.png)  
  
<br>

## 4. 병합 정렬의 성능 분석
### 장점
- `안정성(Stable)`을 보장
- 분할 정복을 쓰는 또 다른 알고리즘 [퀵 정렬(Quick Sort)](퀵%20정렬(Quick%20Sort).md)이 최악의 상황에서 $\mathrm{O(N^2)}$의 성능을 보여주는 것과 달리, 병합 정렬은 항상 $\mathrm{O(N \times logN)}$의 성능을 보여준다.

### 단점
- `N`개의 크기만큼 추가적인 메모리 공간 필요  👉🏻  `비제자리 정렬(Non-in-place sorting)`

### 시간 복잡도
> 최선, 평균, 최악  
- 모두 일정하게 $\mathrm{N \times logN}$ 이라는 시간을 보장
    - `N개의 요소를 가진 배열`을 `1개의 요소를 가진 배열`로 쪼개려면 $\mathrm{logN}$ 과정이 필요
    - `1개의 요소를 가진 배열`이 `N`개가 있으니, `N`번만큼 $\mathrm{logN}$이 호출됨  
