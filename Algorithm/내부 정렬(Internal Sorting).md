# 내부 정렬(Internal Sort)
- 정렬할 데이터를 `메인 메모리(Main memory)`에 올려서 정렬하는 방식  
- 메인 메모리에서 진행하기에, <u>정렬 속도가 빠르다는 장점</u>이 있음  
- 허나, <u>메인 메모리를 초과하는 용량의 데이터에 관해서는 사용할 수 없다</u>는 단점이 존재  
- 내부 정렬은 `비교 기반 정렬`과 `선형 정렬` 방식이 존재함  

<br>

### 1. 비교 기반 정렬  
> 👉🏻 `키(key)`에 대한 사전 정보를 가정하지 않고, <u>오로지 키의 비교에 의해서만 정렬</u>하는 방식  
> 👉🏻 여기서 말하는 `키(key)`란 <u>데이터를 정렬하는 데 사용할 기준이 되는 특정한 값</u>을 의미  

- [버블 정렬 (Bubble Sort)](버블%20정렬(Bubble%20Sort).md)  
- [선택 정렬 (Selection Sort)](선택%20정렬(Selection%20Sort).md)  
- [삽입 정렬 (Insertion Sort)](삽입%20정렬(Insertion%20Sort).md)
- [쉘 정렬 (Shell Sort)](쉘%20정렬(Shell%20Sort).md)
- [퀵 정렬 (Quick Sort)](퀵%20정렬(Quick%20Sort).md)  
- [병합 정렬 (Merge Sort)](병합%20정렬(Merge%20Sort).md)  
- 힙 정렬 (Heap Sort)  

<br>

### 2. 선형 정렬  
> 👉🏻 키에 대한 추가적인 `제약 조건(정보)`를 가정하여 정렬하는 방식  
> 👉🏻 예를 들면, 키 값의 범위, 키의 기수(자리수)와 같은 추가 정보가 주어졌을 때, 이를 정렬에 활용  

- [계수 정렬(Counting Sort)](계수%20정렬(Counting%20Sort).md)
- [버킷 정렬(Bucket Sort)](버킷%20정렬(Bucket%20Sort).md)
- [기수 정렬(Radix Sort)](기수%20정렬(Radix%20Sort).md)  
