## 투 포인터 알고리즘(Two Pointer Algorithm)이란?

- 1차원 배열에서 서로 다른 원소를 가리키고 있는 2개의 포인터를 통해 탐색하는 알고리즘
- 어떤 특정 조건을 만족하는 연속 구간을 구할 때, $\mathrm{O(N)}$으로 구할 수 있음
- 비슷한 테크닉으로 네트워크 패킷 통신에서 사용되는 **`슬라이딩 윈도우(Sliding Window)`** 라는 기법도 존재

### 투 포인터 알고리즘의 작동 원리

- **`start`** , **`end`** 라는 이름으로 2개의 포인터를 준비 
- 항상 start $\leq$ end 의 조건을 만족해야 함.
- 2개의 포인터는 현재 부분 배열의 시작과 끝을 가리킴

### 대표 문제

🔗[BOJ-2470 : 두 용액](https://www.acmicpc.net/problem/2470)  
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;
#define FAST_IO ios::sync_with_stdio(false); cout.tie(NULL); cin.tie(NULL);
#define INF 2000000000

int main()
{
    FAST_IO
    int N;
    cin >> N;

    vector<int> characteristicValues(N);
    for (int i = 0; i < N; i++)
        cin >> characteristicValues[i];
    
    sort(characteristicValues.begin(), characteristicValues.end());
    int start = 0, end = N - 1, standardValue = INF;
    pair<int, int> answer(INF, INF);

    while (start < end)
    {
        int compositeValue = characteristicValues[start] + characteristicValues[end];
        int absCompositeValue = abs(compositeValue);

        if (absCompositeValue < standardValue)
        {
            standardValue = absCompositeValue;
            answer.first = characteristicValues[start];
            answer.second = characteristicValues[end];

            if (absCompositeValue == 0)
                break;
        }

        if (compositeValue > 0)
            end--;
        if (compositeValue < 0)
            start++;
    }

    cout << answer.first << " " << answer.second;
    return 0;
}
```
