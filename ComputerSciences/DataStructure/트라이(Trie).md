## 1. 트라이(Trie)란?

![트라이 예](https://i.namu.wiki/i/PHgJCkRygRzblJVKXeYrtcxtLTliCsMUVtNnHEwliFlzq64-XWJIeIbMIVIlYmdYHrWynFVhrj-SYdeWthGzZZrHUzsgviFcE78yg3loivWWsO3S3R5xtqq8YA25_UAm_qb1DvyawQ456ws_x1oCYw.webp)  
[출처: 나무위키](https://namu.wiki/w/%ED%8A%B8%EB%9D%BC%EC%9D%B4)  

- **`K-진 트리`** 의 구조를 띄고 있으며, **접두사나 단어 자체를 빠르게 탐색할 수 있는 자료구조**
- 사전에서 단어를 찾을 때를 생각해보면, 맨 앞 글자부터 하나씩 찾아가는 걸 떠올릴 수 있음
- 트라이는 이런 방식을 컴퓨터에 맞게 적용시킨 자료구조
- 찾고자 하는 문자열의 길이가 $m$ 이라면, 이게 곧 시간 복잡도가 됨
	- 구현 방법에 따라 시간 복잡도가 조금 달라질 수 있음
- 메모리 공간을 많이 사용하는 대신, 고속으로 탐색할 수 있는 강력함을 자랑

<br>

## 2. 트라이의 구현 방법

### 1) 배열

- 문자열 집합의 개수($n$)와 문자열의 길이($m$)가 작다면 가능한 방법 👉🏻 $\mathrm{O(m)}$
	- 작다의 기준은 **`메모리 사용량 < 256MB`**
- 2차원 배열에서 현재 노드의 위치를 `i`, `j`라고 한다면 `trie[i][j]` 에 접근하는 시간은 $\mathrm{O(1)}$
	- 이를 $m$ 번 반복하기 때문에 $\mathrm{O(m)}$ 의 시간 복잡도가 나오는 것
- 다음 소스 코드는 소문자 알파벳으로만 구성된 단어들을 관리하는 트라이 예
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Node
{
    Node(int N) : children(N, nullptr) { }
    ~Node()
    {
        for (const Node* child : children)
            if (child != nullptr)
                delete child;
    }

    bool isFinished = false;
    vector<Node*> children;
};

class Trie
{
public:
    void Insert(const string& word);
    string Find(const string& word);

    ~Trie();
private:
    void Insert(const string& word, Node* node, int index);
    bool Find(const string& word, Node* node, int index);

private:
    const int WORD_COUNT = 26;
    const char START_CHARACTER = 'a';
    Node* _root = new Node(WORD_COUNT);
};
```  

```cpp
Trie::~Trie()
{
    for (const Node* node : _root->children)
        if (node != nullptr)
            delete node;
}

void Trie::Insert(const string& word)
{
    Insert(word, _root, 0);
}

void Trie::Insert(const string& word, Node* node, int index)
{
    if (index == word.size())
    {
        node->isFinished = true;
        return;
    }

    int charIndex = word[index] - START_CHARACTER;
    if (node->children[charIndex] == nullptr)
        node->children[charIndex] = new Node(WORD_COUNT);
    Insert(word, node->children[charIndex], index + 1);
}

string Trie::Find(const string& word)
{
    return Find(word, _root, 0) ? "YES" : "NO";
}

bool Trie::Find(const string& word, Node* node, int index)
{
    if (index == word.size())
        return node->isFinished;

    int charIndex = word[index] - START_CHARACTER;
    return node->children[charIndex] != nullptr ? Find(word, node->children[charIndex], index + 1) : false;
}
```


### 2) map

- 문자열 집합의 개수($n$)와 문자열의 길이($m$)가 큰 경우에 사용 👉🏻 $\mathrm{O(m \times log(n))}$
- 