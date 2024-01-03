# CHAPTER 06 이진 트리
## CHAPTER 6.1 이진 트리의 정리
- 트리: 루트와 루트의 서브 트리로 구성된 계층형 자료 구조
- 이진 트리 : 최대 2개의 자식 노드를 갖도록 제한하는 트리

## CHAPTER 6.2 이진 트리의 용어
- 노드의 차수 : 특정 노드에 연결된 서브 트리의 수
- 트리의 차수 : 모든 노드의 차수 중 최대 값
- 단말 노드 : 자식 노드가 없는 노드. 차수가 0
- 레벨 : 루트(레벨0)에서 단말 노드 방향으로 단계별로 1씩 증가
- 형제 노드 : 부모가 동일한 자식 노드
- 조상 노드 : 특정 노드에서 루트에 이르는 경로에 존재하는 모든 노드
- 자손 노드 : 특정 노드의 서브 트리에 존재하는 모든 노드
- 내부 노드 : 자식이 최소 1개 이상인 노드
- 외부 노드 : 자식이 없는 단말 노드
- **노드의 높이** : 해당 노드에서 단말 노드에 이르는 가장 긴 경로에서 간선의 수
- **노드의 깊이** : 루트에서 해당 노드에 이르는 경로에서 간선의 수
- 트리의 높이/깊이 : 루트에서 단말 노드에 이르는 가장 긴 경로에서 간선의 수
- 트리의 레벨 : 루트에서 단말 노드로 내려갈 때마다 1씩 증가

#### 프로그램 6.1 트리 노드의 높이와 깊이 계산하기
- 트리와 노드의 높이/깊이를 재귀적 방법으로 계산


```python
# 트리 노드의 높이와 깊이
class Node:
  def __init__(self, data):
    self.data = data
    self.left = None
    self.right = None

class Tree:
  def find(self, root, item): # item 값을 갖는 노드를 찾아 반환 
    if not root: return None
    if root.data == item: return root
    else: 
      node1 = self.find(root.left, item)
      if node1 : return node1
      node2 = self.find(root.right, item)
      if node2 : return node2

  # 노드의 높이
  def height_node(self, root, item):
    node = self.find(root, item)
    if node: return self.height(node)
    else : return None

  # 트리의 높이
  def height(self, root):
    if not root: return -1
    if root.left:
      l_height = 1 + self.height(root.left)
    else: l_height = 0
    if root.right:
      r_height = 1 + self.height(root.right)
    else : r_height = 0

    if l_height >= r_height : return l_height
    else: return r_height

  # 트리의 깊이
  def depth(self, root):
    if not root: return 0
    if root.left: 
      l_depth = 1 + self.depth(root.left)
    else: l_depth = 0
    if root.right:
      r_depth = 1 + self.depth(root.right)
    else: r_depth = 0

    if l_depth >= r_depth: return l_depth
    else: return r_depth

  # 노드의 깊이
  def depth_node(self, root, item):
    if not root: return -1000
    if root.data == item: return 0
    if root.left: 
      l_depth = 1 + self.depth_node(root.left, item)
    else: l_depth = -1000
    if root.right:
      r_depth = 1 + self.depth_node(root.right, item)
    else: r_depth = -1000

    if l_depth >= r_depth: return l_depth
    else: return r_depth

tree = Node('-')
node1 = tree.left = Node('+')
node2 = tree.right = Node('/')
node1.left = Node('*')
node1.right = Node('C')
node2.left = Node('D')
node2.right = Node('E')
node1.left.left = Node('A')
node1.left.right = Node('B')
t = Tree()
print('Tree height', t.height(tree))
for item in ['-', '+', '*', 'C', 'A', 'B', '/', 'D', 'E', 'F']:
  print(item, t.height_node(tree, item))
```

### 이진 트리의 특성

- 이진 트리는 자식 수에 제한이 없는 일반 트리와 달리, **빈 트리**를 허용하며 자식 노드의 **좌우 순서를 구별** 한다
- 이진 트리의 높이는 노드의 최대 레벨로 정한다
- 높이가 h인 이진 트리의 최대 노드 수는 2^(h+1)-1


### 포화 이진 트리
- 포와 이진 트리에서 단말 노드의 수는 차수가 2인 노드의 수에 1을 더한 것과 같다
- n0 = n2 +1
- 노드수가 n개인 포화 이진트리의 높이 : O(log2n)




## CHAPTER 6.3 이진 트리의 표현 
- 이진 트리를 배열로 표현할 때는 완전 이진 트리의 노드 순서와 배열 인덱스를 일치시켜 저장한다
- 인덱스 0번은 비워두고 인덱스 1번부터 노드값 저장 
- 완전 이진 트리 순서로 저장하면 인덱스 [i]에 있는 노드의 부모는 [i/2]에 저장된다
- 노드 [i]의 자식 노드는 [2i]와 [2i+1]에 각각 존재한다


#### 프로그램 6.2 허프만 코딩 트리

































