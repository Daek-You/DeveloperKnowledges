## 이진 탐색(Binary Search)

- **정렬된 데이터들 사이에서 사용할 수 있는 탐색 알고리즘**  👉🏻  $\mathrm{O(log N)}$  
- 술게임 중 소주 뚜껑에 적힌 숫자를 맞추는 **`업 다운(Up-Down) 게임`** 이 예시
	- 0 ~ 100까지 범위를 줬을 때, 보통 반씩 줄여나가며 물어본다.
- **배열에서만 사용 가능한 방식**이기에, [**연결 리스트**](리스트(List).md)와 같은 자료구조에선 사용 불가  👉🏻 ( **`Random Access`** 지원이 안 되므로 )
	- 대신, [**이진 탐색 트리(Binary Search Tree)**](이진%20탐색%20트리(Binary%20Search%20Tree).md) 를 사용해 이진 탐색 적용이 가능

<br>

### 이진 탐색 알고리즘의 동작 과정

> - 제일 왼쪽 데이터 : **`left`**  
> - 제일 오른쪽 데이터 : **`right`**  
> - 찾고자 하는 데이터 : $x$   

<br>


1. 데이터 중앙에 있는 요소, **`mid`** = $(left + right) \ / \ 2$) 를 고른다.
3. $x < \mathrm{mid}$ 라면, **`right`** = $\mathrm{mid} - 1$ 로 탐색 범위를 줄인다.
4. $\mathrm{mid} < x$ 라면, **`left`** = $\mathrm{mid} + 1$ 로 탐색 범위를 줄인다.
5. $x == \mathrm{mid}$ 이거나, **`right < left`** 가 될 때까지 1 ~ 3번 과정을 반복한다.  
<br>

```cpp

template<typename T>
T BinarySearch(const vector<T>& arr, T target) {
	int left = 0, right = arr.size() - 1;

	while (left <= right) {
		int mid = (left + right) / 2;

		if (arr[mid] == target)
			return arr[mid];
		else if (arr[mid] < target)
			right = mid - 1;
		else
			left = mid + 1;
	}
}
```  

<br>

## Lower Bound와 Upper Bound

### Lower Bound

- 데이터들 내에서 특정 값보다 **크거나 같은 값이 처음 나오는 위치**를 리턴  
```cpp
template<typename T>
int LowerBound(const vector<T>& arr, T value) {
	int left = 0, right = arr.size() - 1;

	while (left <= right) {
		int mid = (left + right) / 2;

		if (value <= arr[mid])
			right = mid;
		else
			left = mid + 1;
	}

	return left;
}
```  

### Upper Bound  

- 데이터들 내에서 특정 값보다 **큰 값이 처음으로 나오는 위치**를 리턴
```cpp
template<typename T>
int UpperBound(const vector<T>& arr, T value) {
	int left = 0, right = arr.size() - 1;

	while (left < right) {
		int mid = (left + right) / 2;

		if (value < arr[mid])
			right = mid;
		else
			left = mid + 1;
	}

	return left;
}
```  
