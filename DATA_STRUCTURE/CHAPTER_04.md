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






#### 프로그램 4.2 선형 큐
```python
## 큐의 구현
class Queue:
  def __init__(self, size):
    self.front = -1
    self.rear = -1
    self.size = size
    self.queue = []

  def isEmpty(self):
    return len(self.queue) == 0
  
  def isFull(self):
    return len(self.queue) == self.size
  
  def push(self, item):
    if not self.isFull():
      self.queue.append(item)
      self.rear += 1
      self.show_queue()
    else : print("Queue full")

  def pop(self):
    if not self.isEmpty():
      self.front += 1
      return self.queue.pop(0)
    else : print("Queue empty")

  def show_queue(self):
    print(self.queue)

q = Queue(5)
for item in [3, 4, 5, 6, 7, 8]:
  q.push(item)

for i in range(6):
  q.pop()
  q.show_queue()
```

<img width="275" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/63592ebd-03d8-47b1-9e30-d62e896307b7">



## CHAPTER 4.3 순환 큐

<img width="643" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ceaa61c3-9d0a-47a8-8806-f74ca78cc1dd">

- 순환 큐의 공간에 원소가 모두 채워지면, front와 rear의 변수 값이 같아져 큐의 빈 상태 조건과 구분이 어려워진다
- => 한 칸 남은 상태를 full로 간주하거나, 별도의 count 변수를 사용하여 추가한 원소의 개수를 기억한다
- 아래 코드는 후자

#### 프로그램 4.3 순환 큐

```python
# 순환 큐
class CQueue:
  def __init__(self, size):
    self.size = size
    self.front = 0 # 초깃값 0
    self.rear = 0
    self.cqueue = [None]*self.size # 가변 자료형을 고정 자료형으로 사용하기 위해
    self.count = 0 # 카운트 변수

  def isEmpty(self):
    return self.count == 0 # front = rear도 가능 
  
  def isFull(self):
    return self.count == self.size
  
  def view(self):
    print(self.cqueue, 'F=%2d R=%2d'%(self.front, self.rear))

  def push(self, item):
    if not self.isFull():
      self.count += 1
      print("Push %2d" %item, end = ' ')
      self.rear = (self.rear+1) % self.size
      self.cqueue[self.rear]= item # 큐에 append하면 안됨!!
      self.view()
    else : print("Queue full")

  def pop(self):
    if not self.isEmpty():
      self.count -= 1
      print("Pop   ", end = '   ')
      self.front = (self.front + 1)%self.size
      item = self.cqueue[self.front] # pop 하기 전에 아이템 가져오기
      self.cqueue[self.front] = None
      self.view()
      return item
    else : print("Queue empty")

q = CQueue(5)
for item in [3, 4, 5]:
  q.push(item)
a = q.pop()
q.push(7)
q.push(9)
a = q.pop()
q.push(13)
a = q.pop()
q.push(15)
q.push(8)
q.push(10)
for i in range(6):
  a = q.pop()
```



## CHAPTER 4.4 순환 데크
- **데크** 는 큐의 앞과 뒤 모두에서 원소의 추가와 삭제가 가능한 큐를 말한다
- **순환 데크** 는 데크의 앞과 뒤가 서로 연결된 구조를 말한다



```python
# 순환 데크 

class CDeque:
  def __init__(self, size):
    self.size = size
    self.front = 0 # 초깃값 0
    self.rear = 0
    self.cdeque = [None]*self.size # 가변 자료형을 고정 자료형으로 사용하기 위해
    self.count = 0 # 카운트 변수

  def isEmpty(self):
    return self.count == 0 # front = rear도 가능 
  
  def isFull(self):
    return self.count == self.size
  
  def view(self):
    print(self.cdeque, 'FR',self.front, self.rear)

  def add_rear(self, item):
    if not self.isFull(): # push
      self.count += 1
      print("Add Rear %2d" %item, end = ' ')
      self.rear = (self.rear+1) % self.size
      self.cdeque[self.rear]= item # 큐에 append하면 안됨!!
      self.view()
    else : print("=> Queue full")

  def del_front(self): # pop
    if not self.isEmpty():
      self.count -= 1
      print("Del Front   ", end = '   ')
      self.front = (self.front + 1)%self.size
      item = self.cdeque[self.front] # pop 하기 전에 아이템 가져오기
      self.cdeque[self.front] = None
      self.view()
      return item
    else : print("=> Queue empty")

  def del_rear(self):
    if not self.isEmpty():
      self.count -= 1
      item = self.cdeque[self.rear] # rear니까 삭제사기 >전에< item 받아오기
      self.rear = (self.rear -1 + self.size)%self.size
      print("Del Rear   ", end = '  ')
      self.view()
      return item
    else: print("=> Queue empty")

  def add_front(self, item): 
    if not self.isFull():
      print("Add front  ", end = '  ')
      self.count += 1
      self.cdeque[self.front] = item
      self.front = (self.front -1 + self.size)%self.size
      self.view()
    else: print("=> Queue Full")

    

q = CDeque(5)
for item in [17, 20, 6]:
  q.add_rear(item)
for item in [3, 4, 5]:
  q.add_front(item)
q.del_front()
q.add_front(7)
q.add_rear(9)
q.del_rear()
```

## CHAPTER 4.5 수식 표현과 평가

### 후위 수식 계산하기

#### 후위 수식 알고리즘
```
1. 후위 수식에서 읽은 토큰이 피연산자(숫자)이면 스택에 넣는다
2. 토큰이 연산자이면 스택에서 피연산자 2개를 꺼내서 계산한 후, 그 결과 값을 다시 스택에 넣는다
3. 모든 토큰이 입력되면 스택에 마지막으로 저장된 계산값을 꺼내서 반환한다
```


<img width="354" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/dbdf5aaf-5016-4df0-a02d-77d7188aabd9">



#### 프로그램 4.5 후위 수식 계산하기
```python
# 후위 수식
class Sym:
  OPEN_B = 1 # (
  CLOSE_B = 2 # )
  PLUS = 3 # +
  MINUS = 4 # -
  TIMES = 5 # *
  DIVIDE = 6 # /
  MOD = 7 # %
  OPERAND = 8 # 0~9

class Expression:
  def __init__(self, expr):
    self.stack = []
    self.size = 100
    self.expr = expr
    self.top = -1
  
  def push(self, item):
    self.top += 1
    self.stack.append(item)
    self.show_stack()
  
  def pop(self):
    if len(self.stack) > 0 :  
      self.top -= 1
      return self.stack.pop()
    else: print("Stack Empty")

  def show_stack(self):
    print(self.stack)

  def eval_postfix(self): # 계산하기
    for sym in self.expr:
      print(sym, end = ' ')
      sym_type = self.getSymtype(sym)
      if sym_type == Sym.OPERAND: # 숫자
        self.stack.append(int(sym)) # 문자열이니까 int로 변환해서 append
        print(self.stack)
      else: # 연산자
        op2 =  self.stack.pop() # 순서 주의
        op1 = self.stack.pop()
        if sym_type == Sym.PLUS:
          self.push(op1 + op2)
        elif sym_type == Sym.MINUS:
          self.push(op1 - op2)
        elif sym_type == Sym.TIMES:
          self.push(op1 * op2)
        elif sym_type == Sym.DIVIDE:
          self.push(op1 // op2)
        elif sym_type == Sym.MOD:
          self.push(op1 % op2)
    return self.pop() # 결과값 반환 


  def getSymtype(self, sym):
    if sym == '(': sym_type = Sym.OPEN_B
    elif sym == ')': sym_type = Sym.CLOSE_B
    elif sym == '+': sym_type = Sym.PLUS
    elif sym == '-': sym_type = Sym.MINUS
    elif sym == '*': sym_type = Sym.TIMES
    elif sym == '/': sym_type = Sym.DIVIDE
    elif sym == '%': sym_type = Sym.MOD
    else: sym_type = Sym.OPERAND
    return sym_type


for expr in ["57*9+34/-", "936+5-7*64-*"]:
  e = Expression(expr)
  print("수식 : ", expr)
  print("결과값 =", e.eval_postfix())
  print()
```


### 중위 수식을 후위 수식으로 바꾸기
#### 알고리즘
```
1. 중위 수식의 토큰이 피연산자(숫자)이면 그대로 후위 수식으로 출력한다
2. 토큰이 연산자인 경우는 스택이 비어 있거나, 스택 연산자(stack_top)보다 우선 순위가 높으면 스택에 추가한다
3. 토큰이 스택 연산자보다 우선 순위가 낮거나 같으면, 스택에서 연산자를 먼저 제거한 후 토큰을 스택에 넣는다
4. 토큰이 '('이면, 그대로 스택에 추가한다
5. 토큰이 ')'이면, 스택에서 '('이 나올 때까지 모든 연산자를 제거하여 출력하고 '('은 출력없이 버린다
6. 토큰이 문자열 끝(eos)이면, 스택에 남은 모든 연산자를 출력한다
```

<img width="328" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ef82fd5f-aa30-4a3a-a10a-1d68c45ecdef">



```python
# 중위 수식에서 후위 수식 변환 
class Sym:
  OPEN_B = 1 # (
  CLOSE_B = 2 # )
  PLUS = 3 # +
  MINUS = 4 # -
  TIMES = 5 # *
  DIVIDE = 6 # /
  MOD = 7 # %
  OPERAND = 8 # 0~9

class Expression:
  def __init__(self, expr):
    self.expr = expr
    self.stack = []
    self.size = 100
    self.top = -1
    self.output = ""

  def isFull(self):
    return len(self.stack) == self.size
  
  def isEmpty(self):
    return len(self.stack) == 0
  
  def push(self, item):
    if not self.isFull():
      self.top += 1
      self.stack.append(item)
      self.show_stack()
    else: print("Stack full")

  def pop(self):
    if not self.isEmpty() :  
      self.top -= 1
      return self.stack.pop()
    else: print("Stack Empty")

  def show_stack(self):
    print(self.stack)

  def eval_postfix(self): # 후위 수식 계산 
    for sym in self.expr:
      print(sym, end = ' ')
      sym_type = self.getSymtype(sym)
      if sym_type == Sym.OPERAND: # 숫자
        self.stack.append(int(sym)) # 문자열이니까 int로 변환해서 append
        print(self.stack)
      else: # 연산자
        op2 =  self.stack.pop() # 순서 주의
        op1 = self.stack.pop()
        if sym_type == Sym.PLUS:
          self.push(op1 + op2)
        elif sym_type == Sym.MINUS:
          self.push(op1 - op2)
        elif sym_type == Sym.TIMES:
          self.push(op1 * op2)
        elif sym_type == Sym.DIVIDE:
          self.push(op1 // op2)
        elif sym_type == Sym.MOD:
          self.push(op1 % op2)
    return self.pop() # 결과값 반환 


  def getSymtype(self, sym):
    if sym == '(': sym_type = Sym.OPEN_B
    elif sym == ')': sym_type = Sym.CLOSE_B
    elif sym == '+': sym_type = Sym.PLUS
    elif sym == '-': sym_type = Sym.MINUS
    elif sym == '*': sym_type = Sym.TIMES
    elif sym == '/': sym_type = Sym.DIVIDE
    elif sym == '%': sym_type = Sym.MOD
    else: sym_type = Sym.OPERAND
    return sym_type
  
  def precedence(self, op): # 우선순위
    if op == '(': return 0
    elif op in ['+', '-']: return 1
    elif op in ['*', '/', '%']: return 2

  def infix_postfix(self, expr):
    for token in expr:
      print(token, end = ' ')
      if token.isalnum(): self.output += token # 알파벳 또는 숫자
      elif token == '(': 
        self.push(token)
      elif token == ')': 
        sym = self.pop() # '('이 나올 때까지 모든 연산자 출력
        while sym != '(':
          self.output += sym
          sym = self.pop()
      else: # 연산자
        while not self.isEmpty() and self.precedence(self.stack[-1]) >= self.precedence(token): # token 우선순위가 stak_top 보다 낮으면
          sym = self.pop()
          self.output += sym
        self.push(token) # 위 while문 해당 안될 때 = token우선순위가 더 높을 때 => 스택에 바로 추가
      while not self.isEmpty():
        self.output += self.pop() # 남아있는 모든 원소 출력  
      
  
for expr in ["5*7+(4-3)/6%9", "9/(3+6-5)*7*(6-4)"]:
  e = Expression(expr)
  print('중위 수식 : ', expr)
  print()
  e.infix_postfix(expr)
  print('변환 후위 수식 ', e.output)
  e.stack = []
  e.expr = e.output
  print()
  print('후위 수식 계산 값 =', e.eval_postfix())
  print()
```

- 에러 고쳐야함...


## CHAPTER A Maze Problem


```python
# 미로 탈출
class Maze:
  def __init__(self):
    self.maze = [[1, 1, 1, 1, 1, 1, 1, 1, 1, 1],\
                [1, 0, 0, 1, 0, 0, 0, 0, 1, 1],\
                [1, 1, 0, 1, 0, 1, 1, 0, 0, 1],\
                [1, 0, 0, 0, 0, 1, 1, 1, 1, 1],\
                [1, 0, 1, 1, 0, 1, 0, 0, 0, 1],\
                [1, 0, 0, 1, 0, 1, 1, 0, 1, 1],\
                [1, 1, 0, 0, 1, 0, 0, 0, 1, 1],\
                [1, 1, 1, 0, 0, 0, 1, 0, 0, 1],\
                [1, 0, 0, 0, 1, 0, 1, 1, 'X', 1],\
                [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]]
    self.mark = [[0]*10 for i in range(10)] #  mark[[0]*10]*10] caution! # 갔던 곳 표시하는 배열
    self.stack = [] # 현재까지 경로 
    self.found = False

  def empty(self):
    return len(self.stack) == 0 
  def push(self, row, col):
    self.stack.append((row, col))
  def view(self):
    print("stack", self.stack)

  def explore(self):
    self.mark[1][1] = 1 # 입구
    self.push(1, 1)
    move_next = False # 다음 위치로 이동할지 여부
    while not self.empty() and not self.found: # 스택이 비어있지 않고 출구를 찾지 않은 동안 반복
      if not move_next: # move_next가 False이면
        row, col = self.stack.pop()
      move_next = False
      for x, y in [(0, -1), (1, 0), (0, 1), (-1, 0)]: # N, E, S, W
        next_row = row + x
        next_col = col + y
        if self.maze[next_row][next_col] == 'X': # 탈출 성공
          self.push(row, col)
          self.push(next_row, next_col)
          self.found = True
          break
        elif self.maze[next_row][next_col] == 0 \
        and self.mark[next_row][next_col] == 0: # 미로 열려있음 && 아직 방문 X
          self.mark[next_row][next_col] = 1 # 방문 표시
          print(next_row, next_col, "is visited")
          self.push(row, col)
          row = next_row 
          col = next_col
          move_next = True
          break
    if not self.found: print("No path found")
    else: print("Path found: ")
    self.view()
  
m = Maze()
m.explore()
```




















