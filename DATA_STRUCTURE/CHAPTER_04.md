# CHAPTER 04 스택과 큐 
## CHAPTER 4.1 스택

- 스택 
  - 선형 리스트의 특별한 형태
  - 나중에 들어가는 원소가 가장 먼저 나오는 **후입 선출(LIFO)** 의 구조



- 스택에 원소를 추가하는 연산 : push()
- 스택에서 원소를 삭제하는 연산 : pop()
- push() 전에 stack full 상태인지 검사해야한다
- pop() 전에 스택이 빈 상태가 아닌지 검사해야 한다
- top 변수가 -1이면 스택이 빈 상태임을 의미한다


<img width="559" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ed0dfd61-2642-4bb6-9216-894a4465d7fe">

#### 프로그램 4.1 스택의 구현
```python
# 스택의 구현
class Stack:
  def __init__(self, size):
    self.stack = []
    self.top = -1 # top 값은 배열 index값과 동일
    self.size = size
  
  def push(self, item):
    if self.top < self.size-1: # 인덱스 값 비교니까 size에서 1빼줘야함
      self.top += 1
      self.stack.append(item)
      self.show_stack()
    else: print("Stack full")

  def pop(self):
    if self.top != -1:
      self.top -= 1
      return self.stack.pop()
    else: print("Stack empty")

  def show_stack(self):
    print(self.stack)

s = Stack(5) # 최대 5개까지 들어갈 수 있음 
for i in [3, 4, 5, 6, 7, 8]:
  s.push(i)
for i in range(6):
  s.pop()
  s.show_stack()
```


## CHAPTER 4.2 선형 큐
- 큐
  - 큐는 작업들이 처리되기 전에 대기하고 있는 선형 리스트 자료 구조
  - 큐는 **선입 선출 방식 (FIFO)**



- 선형 큐 : 큐의 시작과 서로 분리되어 있는 구조
- 순환 큐 : 양 끝이 연결된 구조
- front : 큐의 맨앞을 가리키는 변수
  - **주의** 맨 앞 원소 인덱스가 0이면 front -1, 1이면 0, 2이면 1... 
- rear : 큐의 맨 뒤 원소를 가리키는 변수
  
<img width="540" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/4e9c3fc5-9206-47ad-afe2-fa21599ccfc8">























