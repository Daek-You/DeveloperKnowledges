## 1. 삽입 정렬(Insertion Sort)이란?
- 손 안의 카드를 정렬하는 방법과 유사한 정렬 방식
    - 기존의 정렬된 카드 사이에서, 새로운 카드가 들어갈 올바른 자리를 찾아 삽입
    - 새로 삽입될 카드의 수만큼 반복하면, 전체 카드가 정렬됨
- 이전까지의 반복을 통해 이미 정렬된 데이터들의 뒷 부분부터 차례대로 새로운 데이터와 비교를 진행
    - 새로운 데이터가 들어갈 위치를 찾았다면, 해당 위치에 삽입  
<br>

## 2. 삽입 정렬의 알고리즘
크기가 $n$ 인 배열 `arr`가 있고, 정렬 기준이 오름차순이라고 가정  
배열의 인덱스 변수를 `currentIdx` 라고 할 때, 삽입 정렬은 다음과 같은 과정을 거치게 된다.  
<br>


1. `currentIdx = 1`부터 시작하여, `n-1` 까지 차례대로 순회를 진행  
2. `arr[currentIdx]`의 데이터를 별도의 변수 `currentData` 에 저장
3. 각 순회마다 `previousIdx = currentIdx - 1`부터 비교 연산을 다음 조건들이 만족하지 않을 때까지 진행한다.
	- `previousIdx` $\geq 0$ 일 경우
	- `arr[previousIdx] > currentData`일 경우 → `arr[previousIdx+1] = arr[previousIdx]`를 진행 후, `previousIdx--`  
4. 3번 조건에 만족하지 않는다면, `arr[j+1] = currentData`  

![삽입 정렬 시뮬레이션](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/insertion-sort-001.gif)  
출처 : [Tech Interview](https://gyoogle.dev/blog/algorithm/Insertion%20Sort.html)  

<br>

## 3. C++로 구현한 삽입 정렬
```cpp
#include <vector>
using namespace std;

void InsertionSort(vector<int>& arr)
{
    int n = arr.size();

    for (int currentIdx = 1; currentIdx < n; currentIdx++)
    {
        int currentData = arr[currentIdx];
        int previousIdx = currentIdx - 1;

        while ((previousIdx >= 0) and (arr[previousIdx] > currentData))
        {
            arr[previousIdx + 1] = arr[previousIdx];
            previousIdx--;
        }

        arr[previousIdx + 1] = currentData;
    }
}
```  


<br>

## 4. 삽입 정렬의 성능 분석
### 장점
- `안정성(Stable)`을 보장한다.
- `제자리 정렬(in-place sorting)`이다.
- 대부분의 데이터들이 이미 정렬되어 있는 경우, 가장 좋은 성능을 보여준다.  

### 단점
- 데이터들이 역순으로 되어 있는 경우, 최악의 성능을 보여준다.
- 데이터들의 이동 횟수가 많다.
- 데이터들 간 최대 교환 거리가 1이기에, 데이터의 개수가 많을수록 교환 연산이 많이 발생한다.
    - 이것을 보완한 알고리즘이 바로 [쉘 정렬(Shell Sort)](쉘%20정렬(Shell%20Sort).md)    

### 시간 복잡도
> 최선의 경우  
- 데이터들이 이미 정렬되어 있는 경우
- 비교 연산을 한 번씩만 수행하면 끝이므로 $\mathrm{O(n)}$의 성능을 보여준다.  

> 평균 및 최악의 경우  
- 데이터들이 역순으로 배치되어 있는 경우, 최악의 성능을 보여준다.
- 이 경우, 선택 정렬과 마찬가지로 $(n-1)+(n-2)+...+2+1 = \frac{n(n-1)}{2}$의 교환이 일어난다.  →  $\mathrm{O(n^2)}$