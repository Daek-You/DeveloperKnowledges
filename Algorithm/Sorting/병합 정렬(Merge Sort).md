## 1. 병합 정렬(Merge Sort)이란?

![병합 정렬 예](https://media.geeksforgeeks.org/wp-content/uploads/20230706153706/Merge-Sort-Algorithm-(1).png)  

> 1. **배열을 더 작은 하위 배열로 나누고,**  
> 2. **각 하위 배열을 정렬하고,**  
> 3. **정렬된 하위 배열을 다시 병합하여 최종 정렬 배열을 형성하는 방식**  


<br>


- 더 이상 하위 배열로 나눌 수 없을 때까지 계속해서 배열을 반으로 분할하는 재귀 알고리즘
- 배열에 요소가 하나만 남아, 더 이상 분할할 수 없을 때까지 진행
- 그 후, 정렬된 하위 배열이 하나의 정렬된 배열로 병합됨  👉🏻  이 과정에서 **`추가적인 메모리`** 필요  
<br>

## 2. 병합 정렬의 알고리즘
1. **`분할(Divide)` 단계**  
	- 입력 배열을 2개의 크기가 같은 부분 배열로 분할한다.  

2. **`정복(Conquer)` 단계**  
	- 부분 배열을 정렬한다.
	- 부분 배열의 크기가 충분히 작지 않으면, 재귀 호출을 이용하여 다시 분할  

3. **`결합(Combine)` 단계**  
	- 정렬된 부분 배열들을 하나의 배열에 병합한다.  
<br>

다음은 병합 정렬의 시뮬레이션 애니메이션이다.  

![시뮬레이션](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Merge-sort-example-300px.gif/220px-Merge-sort-example-300px.gif)    
<br>

## 3. C++로 구현한 병합 정렬
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> OriginalArray;
vector<int> TempArray;

void Merge(int start, int mid, int last) {
    int left = start, current = start, right = mid + 1;

    // 1. 나뉘어진 두 그룹의 배열에 대해 작은 순서대로 임시 배열(TempArray)에 넣기
    while (left <= mid and right <= last) {
        if (OriginalArray[left] <= OriginalArray[right])
            TempArray[current++] = OriginalArray[left++];
        else
            TempArray[current++] = OriginalArray[right++];
    }

    // 2. 남은 요소들은 그대로 추가해주기
    int k = (left <= mid) ? left : right;
    while (k <= last)
        TempArray[current++] = OriginalArray[k++];

    // 3. 정렬한 내용을 원래 배열에 다시 삽입하기
    for (int i = start; i <= last; i++)
        OriginalArray[i] = TempArray[i];
}

void MergeSort(int left, int right) {
    if (left >= right)
        return;

    int mid = (left + right) / 2;
    MergeSort(left, mid);         // 왼쪽 파티션 분할
    MergeSort(mid + 1, right);    // 오른쪽 파티션 분할
    Merge(left, mid, right);      // 병합
}
```

```cpp
int main() {
    OriginalArray = {38, 27, 43, 3, 9, 82, 10};
    int N = OriginalArray.size();
    TempArray.resize(N);
    
    MergeSort(0, N - 1);

    for (const auto& element : OriginalArray)
        cout << element<< ", ";
    cout << endl;
    
    return 0;
}
```

<br>

## 4. 병합 정렬의 성능 분석
### 장점
- **`안정성(Stable)`** 을 보장
- 분할 정복을 쓰는 또 다른 알고리즘 [**퀵 정렬(Quick Sort)**](퀵%20정렬(Quick%20Sort).md)이 최악의 상황에서 $\mathrm{O(N^2)}$ 의 성능을 보여주는 것과 달리, 병합 정렬은 항상 $\mathrm{O(N \times logN)}$ 의 성능을 보여준다.

### 단점
- $N$ 개의 크기만큼 추가적인 메모리 공간 필요  👉🏻  **`비제자리 정렬(Non-in-place sorting)`**

### 시간 복잡도
> **최선, 평균, 최악**  
- 모두 일정하게 $\mathrm{N \times logN}$ 이라는 시간을 보장
    - **`N개의 요소를 가진 배열`** 을 **`1개의 요소를 가진 배열`** 로 쪼개려면 $\mathrm{log(N)}$ 과정이 필요
    - **`1개의 요소를 가진 배열`** 이 $N$ 개가 있으니, $N$ 번만큼 $\mathrm{logN}$ 이 호출됨  
