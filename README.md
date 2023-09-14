# ComputerScience  

> **목차**  
> 1. [Data Structure](#✏️data-structure)  
> 2. [Algorithm](#✏️algorithm)  
> 3. [Operating System](#✏️operating-system)  


<br>

---
# ✏️Data Structure  
### 1. 기본 개념
- 자료구조의 정의(ADT, Abstract Data Type)와 표현법
- 알고리즘의 정의와 표현
- 알고리즘의 복잡도 계산  

### 2. 선형 자료구조
- 배열 (Array)
- 리스트 (List)
- 스택 (Stack)
- 큐 (Queue)
- 데크 (Deque)  

### 3. 비선형 자료구조
> 트리(Tree)  
- 트리의 개념
- 이진 트리 (Binary Tree)와 순회 (Traversal)
- 수식 트리 (Expression Tree)
- 분리 집합 (Disjoint Set)
- 스레드 이진 트리 (Thread Binary Tree)
- 힙(Heap)  

> 그래프(Graph)  
- 그래프의 개념과 표현
- 기초적인 그래프 연산들
- 최소비용 신장 트리
- 최단 경로
- 작업 네트워크  

<br>

---
# ✏️Algorithm
### 1. 내부 정렬 (Internal Sorting)
👉🏻 정렬할 데이터를 메인 메모리(Main memory)에 올려서 정렬하는 방식  
👉🏻 메인 메모리에서 진행하기에, 정렬 속도가 빠르다는 장점이 있음  
👉🏻 허나, 메인 메모리를 초과하는 용량의 데이터에 관해서는 사용할 수 없다는 단점이 존재  
👉🏻 내부 정렬은 `비교 기반 정렬`과 `선형 정렬` 방식이 존재함  

<br>


> **[ 비교 기반 정렬 ]**  
> 👉🏻 `키(key)`에 대한 사전 정보를 가정하지 않고, <u>오로지 키의 비교에 의해서만 정렬</u>하는 방식  
> 👉🏻 여기서 말하는 `키(key)`란 <u>데이터를 정렬하는 데 사용할 기준이 되는 특정한 값</u>을 의미  

- [버블 정렬 (Bubble Sort)](./Algorithm/버블%20정렬(Bubble%20Sort).md)  
- [선택 정렬 (Selection Sort)](./Algorithm/선택%20정렬(Selection%20Sort).md)  
- [삽입 정렬 (Insertion Sort)](./Algorithm/삽입%20정렬(Insertion%20Sort).md)
- [쉘 정렬 (Shell Sort)](./Algorithm/쉘%20정렬(Shell%20Sort).md)
- 퀵 정렬 (Quick Sort)
- 합병 정렬 (Merge Sort)
- 힙 정렬 (Heap Sort)  

<br>

> **[ 선형 정렬 ]**  
> 👉🏻 키에 대한 추가적인 `제약 조건(정보)`를 가정하여 정렬하는 방식  
> 👉🏻 예를 들면, 키 값의 범위, 키의 기수(자리수)와 같은 추가 정보가 주어졌을 때, 이를 정렬에 활용  

- [계수 정렬(Counting Sort)](./Algorithm/계수%20정렬(Counting%20Sort).md)
- 버킷 정렬(Bucket Sort)
- 기수 정렬(Radix Sort)  

<br>

### 2. 외부 정렬 (External Sorting)
👉🏻 정렬할 데이터를 보조기억장치(HDD, SSD)에서 정렬하는 방식
👉🏻 대용량의 데이터 정렬이 가능
👉🏻 SQL의 order-by, group-by, join 등의 효율적인 처리를 위해 사용
👉🏻 보조기억장치를 쓰기에, 내부 정렬보다는 속도가 떨어짐



<br>

---
# ✏️Operating System

