## 1. 버킷 정렬(Bucket Sort)이란?

![버킷 정렬 예](https://scaler.com/topics/images/bucket-sort-with-integer-elements)   

> 1. **데이터 원소들을 `다양한 그룹(또는 버킷)` 으로 나누고,**
> 2. **버킷마다 정렬한 후,**
> 3. **다시 합치는 방법**   

- **데이터의 분포 특성**을 활용하는 방식
- `버킷의 크기 = 1`이라면, [**계수 정렬**](계수%20정렬(Counting%20Sort).md)과 같다.
- `계수 정렬`은 키 값이 작은 범위 안에 들어올 때 적용할 수 있는 방법
- `버킷 정렬`은 **키 값의 범위뿐만 아니라, 그 범위 내에서 키 값이 확률적으로 균등하게 분포**된다고 가정할 수 있을 때 적용할 수 있는 방법
	- ex) 부동소수점    

<br>

## 2. 버킷 정렬의 알고리즘

1. 원소들의 값 범위에 대해 알아낸 후, 이 범위에 따라 적절한 개수의 비어 있는 버킷을 생성
2. **`분할 단계`** : 입력 배열의 원소들이 각각 속한 범주의 버킷에 저장  
3. **`정렬 단계`** : 각각의 버킷들을 정렬
4. **`수집 단계`** : 버킷을 차례대로 방문하여, 원소들을 원래 배열에 저장  
<br>

## 3. C++로 구현한 버킷 정렬  
```cpp
#include <vector>
#include <algorithm>
#include <list>
#include <cmath>
#define EPSILON 0.00001
using namespace std;

void InsertToBucket(list<float>& lists, float newData)
{
    if (lists.empty())
    {
        lists.push_front(newData);
        return;
    }

    for (auto it = lists.begin(); it != lists.end(); ++it)
    {
        float element = *it;

        // 새로운 데이터가 작거나 같을 경우, 그 앞에 넣으면 자동 정렬
        if (fabs(element - newData) < EPSILON or newData < element)
        {
            lists.insert(it, newData);
            return;
        }
    }

	// for문을 다 돌았는데도 없다면, 제일 큰 수이므로 맨 뒤에 삽입
    lists.push_back(newData);
}


// 0.0 ~ 1.0 사이의 부동소수 정렬 예
void BucketSort(vector<float>& arr, int bucketSize = 10)
{
    int N = arr.size();
    float maxValue = *max_element(arr.begin(), arr.end());
    vector<list<float>> buckets(bucketSize, list<float>());

    // 1. 각 데이터들을 올바른 범주 내의 버킷에 넣는다.
    for (const auto& element : arr)
    {
	    // 소수 첫 번째 자리를 기준으로 0~9번 버킷에 각각 넣는다.
        int index = static_cast<int>(element * bucketSize);
        InsertToBucket(buckets[index], element);
    }


    int index = 0;
    for (int i = 0; i < N; i++)
    {
        while (!buckets[i].empty())
        {
            arr[index++] = *(buckets[i].begin());
            buckets[i].pop_front();
        }
    }
}
```  
<br>

## 4. 버킷 정렬의 성능 분석

### 장점
- 키(key)의 범위가 작고, 균등하게 분포되어 있는 경우에 강력한 성능을 보여준다.  

### 단점
- `비제자리 정렬(Non-in-place sorting)`이다.
- 특정한 상황이 아니라면, 사용할 수 없다.  

### 시간 복잡도

> **최선의 경우 및 평균**  
- **키(key)가 균등하게 분포**되어 있는 경우  👉🏻  $\mathrm{O(N)}$  

> **최악의 경우**  
- **키(key)가 균등하게 분포되지 않아, 한 쪽 버킷에 몰리는 경우**
- 이 경우, 버킷 내부의 데이터를 정렬하는 알고리즘 성능에 의해 버킷 정렬의 성능이 결정됨
