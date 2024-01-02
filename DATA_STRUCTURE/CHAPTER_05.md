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























