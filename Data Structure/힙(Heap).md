# 힙(Heap)  

> **목차**  
> 1. [힙 자료구조란?](#1-힙heap-자료구조란)  
> 1.1. [힙의 종류](#힙heap의-종류)  
> 2. [우선순위 큐란?](#2-우선순위-큐priority-queue란)  
> 3. [힙의 구현](#3-힙heap-구현)  


<br>

## 1. 힙(Heap) 자료구조란?
- `완전 이진 트리(Complete Binary Tree)`의 일종으로, `우선순위 큐(Priority Queue)`를 위해 만들어진 자료구조
- 최솟값과 최댓값을 쉽게 추출할 수 있도록 설계되어 있다.  
- 중복된 값을 허용 (이진 탐색 트리에서는 허용하지 않음)  

### 힙(Heap)의 종류  
> **최대 트리(Max tree)와 최대 힙(Max heap)**
- `최대 트리(Max tree)` : "트리의 모든 노드"에 대해, **노드의 데이터 값 $\geq$ 자식 노드의 데이터 값**을 만족하는 트리
- `최대 힙(Max heap)` : `최대 트리`이면서, `완전 이진 트리`  

    ![최대 힙 예](../Resources/Images/최대%20힙%20예.png)  


<br>

> **최소 트리(Min tree)와 최소 힙(Min heap)**  
- `최소 트리(Min tree)` : "트리의 모든 노드"에 대해, **노드의 데이터 값 $\leq$ 자식 노드의 데이터 값**을 만족하는 트리  
- `최소 힙(Min heap)` : `최소 트리`이면서, `완전 이진 트리`  

    ![최소 힙 예](../Resources/Images/최소%20힙%20예.png)  

<br>

## 2. 우선순위 큐(Priority Queue)란?
- 선입선출(First-In, First-Out) 방식이 아닌, <u>우선순위의 순서대로 삭제되는 큐</u>  

    > ex) 시뮬레이션 시스템, 비행기 탑승 대기, 운영체제의 작업 스케줄링, 네트워크 트래픽 제어, 수치 해석적인 계산 등...  

- 우선순위 큐는 `배열`, `연결리스트`, `힙`으로 구현이 가능  
    - 그 중에서도 `힙(Heap)`으로 구현하는 것이 가장 효율적  

        ![우선순위 큐를 표현하는 방법](https://gmlwjd9405.github.io/images/data-structure-heap/data-structure-heap-priorityqueue.png)  

<br>

## 3. 힙(Heap) 구현
- 힙은 배열로 구현
.. 계속 작성




