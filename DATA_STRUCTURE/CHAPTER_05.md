# CHAPTER 05 연결 리스트
## CHAPTER 5.1 연결 리스트 개요

- 연결리스트는 노드들이 링크에 의해 순차적으로 연결된 자료구조
- 노드는 값을 저장하는 공간과 노드를 연결하는 링크로 구성된다
- 단일 연결 리스트(sll) : 노드들이 한 방향으로만 연결된 구소
- 이중 연결 리스트(dll) : 노드들이 앞 뒤 양 방향으로 연결된 구조

<img width="542" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/2acc26b5-567f-4718-984f-79d0071a12c1">

## CHAPTER 5.2 단일 연결 리스트

- head 변수 : 연결 리스트의 첫 노드를 가리킨다
- tail 변수 : 연결 리스트의 마지막 노드를 가리키다
- 마지막 노드의 링크 값은 None이며 이를 통해 연결 리스트의 끝을 알 수 있다

### 연결 리스트 노드 추가
- 연결 리스트에 노드를 추가하는 연산은 연결 리스트가 비어 있는 그렇지 않은 상태를 각각 고려해야 한다
- attach() 함수 : 노드를 리스트 뒤에 추가하는 연산 구현

#### 프로그램 5.1 단일 연결 리스트 생성
```python
# 3개의 노드로 연결 리스트 생성
class Node:
  def __init__(self, item):
    self.data = item
    self.link = None

# 노드들을 관리하는 리스트
class SinglyLinkedList:
  def __init__(self):
    self.head = None
    self.tail = None
  
  def isEmpty(self):
    return not self.head # head가 None이면 Empty
  
  def view(self):
    temp = self.head # 맨앞 (head) 노드를 가리키는 변수 
    print("[", end = ' ')
    while temp : 
      print(temp.data, end = ' ') # 현재 노드 데이터 출력 
      temp = temp.link # 다음 노드로 이동 
    print("]", end = ' ')
    if self.head:  # head 존재 = 빈 리스트 아님
      print("H=", self.head.data, "T=", self.tail.data)
    else:
      print("빈 리스트")
  
  def attach(self, item): # 리스트 뒤에 추가
    node = Node(item)
    if not self.head: # empty이면
      self.head = node # head와 tail을 새로운 노드로 설정 
      self.tail = node
    elif self.tail: # tail 존재 = empty아님 = 기존에 하나 이상 있는 경우 
      self.tail.link = node # 기존 tail의 link를 새로운 노드로 설정
      self.tail = node # tail을 새로운 노드로 업데이트 

list1 = SinglyLinkedList()
for item in [100, 200, 300]:
  list1.attach(item)
  list1.view()
```


<img width="511" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9f486d1b-5bcf-42b9-943a-6934e43d3800">



#### 프로그램 5.2 연결 리스트 노드 추가
```python
  # 연결 리스트에서 노드 찾기 
  def find(self, item): # 데이터가 'item'인 노드를 찾아 그 이전 노드와 해당 노드 반환 
    if not self.head: return None, None # 빈 리스트일 경우 None 반환

    temp = self.head # temp = head 노드 가리키는 변수
    prev = None # 이전 노드 가리키는 prev 변수 초기화
    while temp: 
      if temp.data == item:
        return prev, temp # 이전 노드, 현재 노드 반환
      prev = temp # !!!! 다음 노드로 이동하기 전에 prev를 현재 노드로 설정 
      temp = temp.link # 다음 노드로 이동 
    return None, None # 노드 찾지 못한 경우 None 반환 

# 노드 삽입 
  def insert(self, prev, item):
    if not self.head: return # 빈 리스트일 경우 삽입 불가

    node = Node(item) # 새로운 노드 생성
    if not prev: # 이전 노드 없음 (= 맨 앞에 삽입)
      node.link = self.head # 새로운 노드의 링크를 기존 head로 설정
      self.head = node # head를 새로운 노드로 설정
      return
    
    before, temp = self.find(prev) # 이전 노드 찾기 
    if not temp: # 이전 노드 없는 경우 
      print("앞 노드 없음")
      return
    else: # 중간에 삽입 
      node.link = temp.link
      temp.link = node
      if not node.link :  # 새로운 노드가 마지막 노드인 경우 
        self.tail = node # tail을 새로운 노드로 업데이트 
```

### 연결 리스트 노드의 삭제 

#### 프로그램 5.3 연결 리스트 노드의 삭제 
```python
# 리스트의 노드 삭제하기
def del_find(self, item): # data값이 item인 노드 찾아서 삭제하기
  if not self.head:
    print("빈 리스트")
    return 
  prev, node = self.find(item)
  if not node:
    print("삭제 노드 없음")
    return
  if prev:
    prev.link = node.link
    print("노드 삭제됨")
  else: # no prev
    if self.head == node: # 맨 앞 노드 삭제
      self.head = node.link
      print("첫 노드 삭제됨 ")
  if node == self.tail:
    self.tail = prev
  del node 
```




#### 프로그램 5.4 단인 연결 리스트 전체 코드 
```python
class Node:
  def __init__(self, item):
    self.data = item
    self.link = None

class SLL:
  def __init__(self):
    self.head = None
    self.tail = None

  def isEmpty(self):
    return not self.head
  
  def view(self):
    temp = self.head
    print("[", end = ' ')
    while temp:
      print(temp.data, end = ' ')
      temp = temp.link
    print("]", end = ' ')
  
  def attach(self, item): 
    node = Node(item)
    if not self.head:
      self.head = node
      self.tail = node
    elif self.tail:
      self.tail.link = node
      self.tail = node

  def find(self, item):
    if not self.head: return None, None

    temp = self.head
    prev = None
    while temp:
      if temp.data == item:
        return prev, temp
      prev = temp
      temp = temp.link
    return None, None
  
  def insert(self, prev, item):
    if not self.head:
      return
    
    node = Node(item)
    if not prev: # 맨 앞에 추가
      node.link = self.head
      self.head = node
      return 
    before, temp = self.find(prev)
    if not temp: 
      return 
    else: 
      node.link = temp.link
      temp.link = node
      if not node.link:
        self.tail = node 
        # 리스트의 노드 삭제하기
  def del_find(self, item): # data값이 item인 노드 찾아서 삭제하기
    if not self.head:
      print("빈 리스트")
      return 
    prev, node = self.find(item)
    if not node:
      print("삭제 노드 없음")
      return
    if prev:
      prev.link = node.link
      print("노드 삭제됨")
    else: # no prev
      if self.head == node: # 맨 앞 노드 삭제
        self.head = node.link
        print("첫 노드 삭제됨 ")
    if node == self.tail:
      self.tail = prev
    del node

lst = SLL()
for item in [100, 200, 300]:
  lst.attach(item)
  lst.view()
lst.insert(100, 150)
lst.view()
lst.insert(150, 180)
lst.view()
lst.insert(300, 500)
lst.view()
lst.insert(400, 500)
lst.view()
for item in [400, 200, 100, 300, 500, 150, 180]:
  print(item, end = ' ')
  lst.del_find(item)
  lst.view()
```

## CHAPTER 5.3 연결 리스트 연산
### 연결 리스트 길이 세기
- 연결 리스트의 길이는 노드의 수를 의미한다

```python
# 연결 리스트의 길이
def length(self):
  count = 0
  temp = self.head
  while temp:
    count += 1
    temp = temp.link
  return count
```



### 리스트 전체 삭제 
```python
# 리스트 전체 삭제
def del_list(self):
  node = self.head
  while node:
    temp = node
    node = node.link
    del temp 
  print("list is deleted")
  self.head = None
```




### add_sort()
```python
def add_sort(self, item):
  node = Node(item)
  if not self.head:
    self.head = None
    self.tail = None
    return
  
  temp = self.head
  prev = None
  # 새로운 노드의 적절한 위치를 찾기 위해 연결 리스트를 순회 
  while temp:
    if item > temp.data:
      prev = temp # 다음 노드로 이동 
      temp = temp.link
    elif item == temp.data: # 아이템이 이미 연결 리스트에 존재
      print("Duplicate item") 
      return
    else: # item < temp.data
      node.link = temp # 새로운 노드를 현재 노드(temp) 앞에 추가 
      if prev: prev.link = node
      else: self.head = node
      return 
  prev.link = node
  self.tail = node
```

### 리스트 합치기
- 두 개의 리스트를 연결하여 하나로 합치는 것을 결합(concatenation)이라고 한다


#### 프로그램 5.6 연결 리스트 합치기
```python
# 연결 리스트 합치기
def concat(self, second):
  if not self.head: # 첫번째 리스트 비어있음
    return second
  elif second: # 두 리스트 모두 노드 있음
    temp = self.head
    while temp.link: # 첫번째 리스트 마지막 노드 찾기
      temp = temp.link
    temp.link = second
  return self.head

first.concat(second) # first에 second 붙이기 
```
- 시간 복잡도 : **O(length of first list)**




## CHAPTER 5.4 순환 연결 리스트
- 마지막 노드의 링크가 None이 아니고 그 리스트의 첫 노드에 다시 연결되어 있으면 순환 연결 리스트라고 한다


- 순환 연결 리스트는 링크가 None인 마지막 노드가 없기 때문에 시작 노드로 다시 되돌아오는 순간이 전체 리스트를 모두 탐색한 시점이다


#### 프로그램 5.7 순환 연결 리스트
```python
# 순환 연결 리스트
class Node:
  def __init__(self, item): 
    self.data = item
    self.link = None

  class CircularList:
    def __init__(self): 
      self.head = None
      self.tail = None

    def isEmpty(self): return not self.head

    def add_rear(self, item): # 리스트 뒤에 노드 추가
      node = Node(item)
      if not self.head:
        self.head = node
        self.tail = node
      elif self.tail:
        self.tail.link = node
        self.tail = node
      node.link = self.head

    def length_list(self): # 리스트 길이 세기
      if not self.head: return 0
      count = 0
      temp = self.head
      while True:
        count += 1
        temp = temp.link
        if temp == self.head : break # 중요!!!!
      return count
    
    def add_sort(self, item): # add a node to circular list orderly
      node = Node(item)
      if not self.head: 
        self.head = node
        self.tail = node
        node.link = self.head
      
      temp = self.head
      prev = None
      while temp:
        if item > temp.data: 
          prev = temp
          temp = temp.link
        elif item == temp.data:
          print("Duplicate item")
          return
        else: 
          node.link = temp
          if prev: prev.link = node
          else: 
            self.head = node
            self.tail.link = node
          return
        if temp == self.head : break
      node.link = self.head
      prev.link = node
      self.tail = node
```





### 역순 연결 리스트 
- 역순 연결 리스트란 주어진 리스트의 노드들을 반대 방향으로 연결한 리스트를 말한다
- 시간 복잡도 : O(노드의 수)

<img width="424" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/fd470530-4372-4c26-8a30-4c92ff44df32">


#### 알고리즘
```
1. third 변수가 second 노드를 가리키게 한다
2. second 변수가 firt 노드를 참조하게 한다
3. first 변수를 다음 노드로 이동시킨다
4. second 노드의 링크에 third 값을 저장한다
5. firt가 None이 아닐 때까지 1번부터 반복한다
```

#### 프로그램 5.8 역순 연결 리스트
```python
# 역순 연결 리스트
def reverse(self):
  first = self.head
  second = third = None
  self.tail = self.head
  while first: 
    third = second
    second = first
    first = firsts.link
    second.link = third

  self.head = second
  return 
```
-  first, second, third를 이용하여 현재 노드(first)의 링크를 이전 노드(second)로 바꾸고, 각 노드를 한 칸씩 이동하며 반복. 최종적으로 self.head를 역순으로 조정된 리스트의 시작으로 설정


## CHAPTER 5.5 이중 연결 리스트
- 이중 연결 리스트는 각 노드에서 앞 뒤 노드로 링크가 연결되어 있다
- 이중 연결 리스트는 이진 트리를 구현하는데 활용된다
- 데이터, 왼쪽 링크, 오른쪽 링크 필드로 구성


<img width="419" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/327d7896-972d-4705-a96e-a4223394317b">



### 이중 연결 리스트의 표현
- 이중 연결 리스트에는 **헤드 노드** 존재
- 단일 연결 리스트의 head 변수와는 다른 것이다
- 헤드 노드의 링크는 리스트의 첫 노드와 마지막 노드를 가리킨다
- 헤드 노드의 데이터 필드에는 **전체 노드의 수**를 저장한다

<img width="418" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/45741e11-ce78-4a86-9489-58088c5b0d65">


#### 프로그램 5.9 이중 연결 리스트의 선언문
- 이중 연결 리스트의 초기 상태(empty)에서 헤드 노드의 rlink와 llink는 헤드 노드 사진을 가리킨다


```python
# 이중 연결 리스트 선언문
class Node:
  def __init__(self, item):
    self.data = item
    self.llink = None
    self.rlink = None
class Linkedlist:
  def __init__(self):
    self.head = Node(0) # 노드수 0
    self.head.rlink = self.head
    self.head.llink = self.head

  def empty(self):
    return self.head.rlink == self.head
```


### 이중 연결 리스트에 노드 추가

```python
# dll : add
def add(self, item): # 맨 뒤에 추가
  node = Node(item)
  self.head.data += 1
  if self.empty(): 
    node.llink = self.head
    node.rlink = self.head
    self.head.rlink = node
    self.head.llink = node
  else:
    node.llink = self.head.llink
    node.rlink = self.head
    self.head.llink.rlink = node
    self.head.llink = node
```

### 이중 연결 리스트의 노드 삭제
```python
def delete(self, item):
  if self.empty():
    print("List Empty")
    return
  node = self.find(item)
  if not node:
    print("Not found")
    return
  if node == self.head: # head 노드 삭제하려고 하는 경우
    print("head is not deleted")
    return
  self.head.data -= 1
  node.llink.rlink = node.rlink
  node.rlink.llink = node.llink
  del node
```





















