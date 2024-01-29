## 1. 트라이(Trie)란?

![트라이 예](https://i.namu.wiki/i/PHgJCkRygRzblJVKXeYrtcxtLTliCsMUVtNnHEwliFlzq64-XWJIeIbMIVIlYmdYHrWynFVhrj-SYdeWthGzZZrHUzsgviFcE78yg3loivWWsO3S3R5xtqq8YA25_UAm_qb1DvyawQ456ws_x1oCYw.webp)  
[출처: 나무위키](https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4)  

- **`K-진 트리`** 의 구조를 띄고 있으며, **접두사나 단어 자체를 빠르게 탐색할 수 있는 자료구조**
- 사전에서 단어를 찾을 때를 생각해보면, 맨 앞 글자부터 하나씩 찾아가는 걸 떠올릴 수 있음
- 트라이는 이런 방식을 컴퓨터에 맞게 적용시킨 자료구조
- 찾고자 하는 문자열의 길이가 $m$ 이라면, 이게 곧 시간 복잡도가 됨
	- 구현 방법에 따라 시간 복잡도가 조금 달라질 수 있음
- 메모리 공간을 많이 사용하는 대신, 고속으로 탐색할 수 있는 강력함을 자랑
- 접두사를 얻을 수 있음


## 2. 트라이의 구현 방법

### 1) 배열

- 문자열 집합의 개수($n$)와 문자열의 길이($m$)가 작다면, `배열`을 통해 구현 가능
	- 작다의 기준은 **`메모리 사용량 < 256MB`**
	- 알파벳 단어들(a ~ z 또는 A ~ Z), 숫자(0 ~ 10)
- 다음 소스 코드는 소문자 알파벳으로만 구성된 단어들을 관리하는 트라이 예([BOJ - 5052](https://www.acmicpc.net/problem/5052))
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Node
{
    Node(int N) : children(N, nullptr), isFinished(false), hasChild(false) { }
    ~Node()
    {
        for (Node* child : children)
            if (child != nullptr)
                delete child;
    }

    bool isFinished;
    bool hasChild;
    vector<Node*> children;
};

class Trie
{
public:
    Trie(int size, char startCharacter) : _size(size), _startCharacter(startCharacter)
    {
        _root = new Node(size);
    }

    ~Trie() { delete _root; }

    void Insert(const string& word);
    bool ContainsPrefixWord(const string& word)
    { 
	    return ContainsPrefixWord(word, _root, 0);
	}

private:
    bool ContainsPrefixWord(const string& word, Node* currentNode, int wordIndex);

private:
    const int _size;
    const char _startCharacter;
    Node* _root = nullptr;
};
```  

```cpp
void Trie::Insert(const string& word)
{
    Node* currentNode = _root;

    for (const char& character : word)
    {
        int index = character - _startCharacter;
        if (currentNode->children[index] == nullptr)
        {
            currentNode->children[index] = new Node(_size);
            currentNode->hasChild = true;
        }
        
        currentNode = currentNode->children[index];
    }

    currentNode->isFinished = true;
}

bool Trie::ContainsPrefixWord(const string& word, Node* currentNode, int wordIndex)
{
    // 끝까지 다 찾았는데, 현재 단어를 접두사로 가지는 더 긴 단어가 없는 경우
    if (wordIndex >= word.length() and !currentNode->hasChild)
        return false;

    // 끝까지 다 찾았는데, 현재 단어를 접두사로 가지는 더 긴 단어가 있는 경우
    if (wordIndex >= word.length() and currentNode->hasChild)
        return true;

    // 현재 단어보다 짧은 접두사 단어가 존재하는 경우
    if (currentNode->isFinished)
        return true;

    int index = word[wordIndex] - _startCharacter;
    return ContainsPrefixWord(word, currentNode->children[index], wordIndex + 1);
}
```  

### 2) 이진 검색 트리, 해시

- $n$과 $m$이 크다면 **`BST(std::map)`** 나 **`해시(unordered_map)`** 와 같은 자료구조를 활용
- 다음 글자를 Key, 그에 대응되는 노드 주소를 Value로 하여 관리

```cpp
작성 중...
```


## 3. 트라이의 성능 분석

문자열 집합의 개수를 $N$, 찾고자 하는 문자열의 길이를 $M$이라고 한다면 다음과 같다.
- 배열을 사용할 경우, 문자열 길이만큼 시간이 걸림 👉🏻 $\mathrm{O(M)}$
- 이진 검색 트리를 활용할 경우, 탐색 시간 $\mathrm{O(logN)}$이 문자열 길이만큼 반복 👉🏻 $\mathrm{O(M \times log(N))}$
- 해시를 활용할 경우, 해시 충돌이 일어나지 않는다면 배열과 비슷한 성능을 보여줌