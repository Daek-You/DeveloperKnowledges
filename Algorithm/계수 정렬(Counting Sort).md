# 계수 정렬(Counting Sort)

> **목차**  
> 1. [계수 정렬이란?](#1-계수-정렬counting-sort이란)  
> 2. [계수 정렬의 알고리즘](#2-계수-정렬의-알고리즘)  
> 3. [C++로 구현한 계수 정렬](#3-c로-구현한-계수-정렬)  
> 4. [계수 정렬의 성능 분석](#4-계수-정렬의-성능-분석)  

<br>

## 1. 계수 정렬(Counting Sort)이란?
- $n$개의 데이터들 중 최댓값을 `K`라고 할 때, `0 ≤ 원소들의 값 ≤ K` 일 때에만 사용 가능한 알고리즘
- 정렬할 각 데이터의 빈도 수를 기반으로 동작
- K 값이 작다면, 퀵 정렬보다도 빠른 성능을 보여준다.
- K 값이 커진다면, 매우 나쁜 성능을 보여준다.  

<br>

## 2. 계수 정렬의 알고리즘  
> 1. `입력 배열의 데이터들 개수를 셀 배열`과 `정렬 결과를 저장할 배열` 2개가 필요  
> 2. 입력 배열에서 가장 큰 데이터 `K`를 찾는다.  
> 3. $K+1$ 크기의 `카운팅 배열`을 선언한 후, `입력 배열`을 돌며 개수를 센다.  
> 4. `카운팅 배열`을 순회하며, `누적합`을 카운팅 배열에 다시 저장해 나간다.  
> 5. `입력 배열`을 뒤에서부터 순회하며, 다음을 진행한다.  
    👉🏻 `입력 배열의 원소`를 `카운팅 배열의 인덱스`로 하여, 카운팅 배열에서 값을 읽어옴  
    👉🏻 `해당 값 - 1`이 `정렬 결과 배열에서의 인덱스 위치`  
    👉🏻 `정렬 결과 배열[해당값 - 1]`에 `입력 배열의 원소`를 넣는다.  

<br>

> [🔗계수 정렬 시뮬레이션 사이트](https://www.cs.miami.edu/home/burt/learning/Csc517.091/workbook/countingsort.html)  

<br>


## 3. C++로 구현한 계수 정렬
```cpp
#include <vector>
#include <algorithm>
using namespace std;

void CountingSort(const vector<int>& origin, vector<int>& result)
{
    /* 원소들 중 음수 값이 있다면, 에러 처리 */

    int N = origin.size();

    // 1. 배열에서 가장 큰 원소 값 K를 찾은 후, K+1 크기의 배열 생성
    int K = *max_element(origin.begin(), origin.end());
    vector<int> cumulativeSumCount(K + 1, 0);
    
    // 2. 입력 배열을 순회하며, 각 원소의 개수를 카운팅
    for (const auto& element : origin)
        cumulativeSumCount[element]++;

    // 3. 카운팅 개수를 누적하여 저장
    for (int i = 1; i <= K; i++)
        cumulativeSumCount[i] += cumulativeSumCount[i - 1];

    // 4. 뒤에서부터 입력 배열의 원소를 누적합 카운팅 배열의 인덱스로 사용하여 배치 될 위치 찾기
    //      - 뒤에서부터 읽는 이유는 Stable을 보장하기 위함
    //      - 원래 인덱스 자리를 위해 -1를 적용한 곳에 배치
    for (int i = N - 1; i >= 0; i--)
    {
        int element = origin[i];
        int index = --cumulativeSumCount[element];
        result[index] = element;
    }
}
```  

<br>

## 4. 계수 정렬의 성능 분석
### 장점
- `안정성(Stable)`을 보장
- 비교와 교환 연산이 필요하지 않음
- `K`가 `N`보다 작거나 비슷할 때, $\mathrm{O(N + K)}$ 이라는 엄청난 성능을 보여줌
    - 입력 배열의 각 데이터를 카운팅하는 데에 $\mathrm{O(K)}$, 정렬 결과를 담을 배열에서 각 데이터의 올바른 인덱스 값을 찾는데에 $\mathrm{O(N)}$ 
- `K`의 크기가 `N`과 비슷하다면, 시간 복잡도와 공간 복잡도 모두 $\mathrm{O(N)}$에 가능
- 점수, 알파벳과 같이 <u>좁은 범위의 데이터를 정렬할 때</u> 매우 유용  

### 단점
- `K` 값에 영향을 크게 받는 알고리즘  →  K = $N^3$ 라면, $\mathrm{O(N^3)}$의 복잡도를 가짐  

    ```
    입력 배열이 [1, 1000000]이라면?
    원소는 2개밖에 없는데, 1000001개 크기의 카운팅 배열을 생성해야 함 → 엄청난 메모리 낭비
    ```
- 따라서, `K`의 값이 `N`보다 훨씬 큰 경우에는 굉장히 성능이 나빠질 수 있음