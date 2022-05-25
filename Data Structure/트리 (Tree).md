# 트리 (Tree)

> 트리는 노드로 이루어진 자료 구조

![img](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F4680ea5f-ce25-492c-9358-c780bb98611d%2Fimage.png)

![img](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Ffefd2e62-bc4f-427a-a364-2ebc0b0c5c70%2Fimage.png)

- `트리`는 그래프의 일종으로 정점과 간선을 이용하여 데이터의 배치 형태를 추상화한 자료구조
- `트리`는 서로 다른 두 노드를 연결하는 길이 하나뿐인 그래프
- 힙을 구현하는 방법 중 하나가 `트리`

----

`트리`는 노드로 이루어진 자료 구조

1. `트리`는 하나의 루트 노드를 갖는다.
2. 루트 노드는 0개 이상의 자식 노드를 갖고 있다.

3. 그 자식 노드 또한 0개 이상의 자식 노드를 갖고 있고, 이는 반복적으로 정의 된다.

----

## 용어

![img](https://gmlwjd9405.github.io/images/data-structure-tree/tree-terms.png)

- 루트 노드 (root node) : 부모가 없는 노트, 트리는 하나의 루트 노드만 가진다.
- 단말 노드 (leaf node) : 자식이 없는 노드, '말단 노드' 또는 '잎 노드' 라고 부른다.
- 내부 노드 (internal node) : 단말 노드가 아닌 노드
- 간선 (edge) : 노드를 연결하는 선 (link, branch 라고도 부름)
- 형제 (sibling) : 같은 부모를 가지는 노드
- 노드의 크기 (size) : 자신을 포함한 모든 자식 노드의 개수
- 노드의 깊이 (depth) : 루트에서 어떤 노드에 도달하기 위해 거쳐야 하는 간선의 수
- 노드의 레벨 (level) : 트리의 특정 깊이를 가지는 노드의 집합
- 노드의 차수 (degree) : 하위 트리 개수 / 간선 수(degree) = 각 노드가 지닌 가지의 수
- 트리의 차수 (degree of tree) : 트리의 최대 차수
- 트리의 높이 (height) : 루트 노드에서 가장 깊숙히 있는 노드의 깊이



> 노드란 재분배 지점 또는 통신 종단점
>
> 노드란 매듭, 절, 집합점, 중심점
>
> **노드란 데이터 지점을 의미**



## 특징

- `트리` 자료구조는 일반적으로 대상 정보의 각 항목들을 계층적으로 구조화할 때 사용하는 비선형 자료구조
- `트리`의 구조는 '저장된 데이터를 더 효과적으로 탐색' 하기 위해서 사용
- `트리`는 `리스트` 와 다르게 데이터가 나열되는 구조가 아닌 부모(parent)와 자식(child)의 계층적인 관계로 표현
- `트리`는 사이클이 없음
- `트리`에서 루트노드를 제외한 모든 노드는 단 하나의 부모노드를 갖는다.
- 노드(node) 들과 노드들을 연결하는 간선(edge) 들로 구성되어 있다.



## 종류



### 이진 트리 (Binary Tree)

> 각 노드가 최대 두 개의 자식을 갖는 트리

각 노드가 최대 두 개 자식을 갖는 트리이다.

모든 트리가 이진 트리는 아니다.



#### 순회

`트리`의 순회란 `트리`의 각 노드를 체계적인 방법으로 탐색하는 과정을 의미

노드를 탐색하는 순서에 따라 전위 순회, 중앙 순회, 후위 순회 3가지로 분류



##### 전위 순회 (Preorder)

![img](https://upload.wikimedia.org/wikipedia/commons/a/ac/Preorder-traversal.gif)

루트노드 -> 왼쪽 서브트리 -> 오른쪽 서브트리 의 순서로 순회하는 방식

깊이 우선 순회 라고도 불린다.



##### 중위 순회 (Inorder)

![img](https://upload.wikimedia.org/wikipedia/commons/4/48/Inorder-traversal.gif)

왼쪽 서브트리 -> 노드 -> 오른쪽 서브트리 의 순서로 순회하는 방식

대칭 순회 라고 불린다.



##### 후위 순회 (Postorder)

![img](https://upload.wikimedia.org/wikipedia/commons/2/28/Postorder-traversal.gif)

왼쪽 서브트리 -> 오른쪽 서브트리 -> 노드 의 순서로 순회하는 방식



### 이진 탐색 트리 (Binary Search Tree)

모든 노드가 아래와 같은 특정 순서를 따르는 속성이 있는 이진 트리

`모든 왼쪽 자식들 <= n < 모든 오른쪽 자식들 (모든 노드 n에 대해서 반드시 true)`





----

참고사이트

velog : https://velog.io/@kimdukbae/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%A6%AC-Tree

github : https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html