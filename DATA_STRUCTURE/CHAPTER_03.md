# CHAPTER 03 재귀 호출
## CHAPTER 3.1 재귀 호출의 개념
- 재귀 호출 : 함수의 실행 중에 자신을 다시 호출하는 상황

#### 프로그램 3.1 재귀문 팩토리얼
```python
# 재귀문 팩토리얼
def fact(n):
  if n==0: return 1
  else : return n*fact(n-1)

i = 20
for n in range(10, i+1):
  print(n, "!=", fact(n))
```

#### 프로그램 3.2 최대 공약수
```python
# 최대 공약수 구하기
def gcd(x, y):
  if y == 0:
    return x
  else: return gcd(y, x%y)

num = [(128, 12), (45, 120)]
for x, y in num:
  print("gcd(%d, %d) = %d" %(x, y, gcd(x,y))  )
```

## CHAPTER 3.2 이진 탐색
#### 프로그램 3.3 반복문 이진 탐색
```python
# 반복문 이진 탐색
def binary_search(lst, item, left, right): # left, right : 인덱스 번호
  while left <= right:
    mid = (left+right)//2 # 중간 값 인덱스 번호
    if item == lst[mid]:
      return mid # 여기는 return문!
    elif item < lst[mid]:
      right = mid -1
    else: # item > lst(mid)
      left = mid +1
  return -1

mylist = [5, 8, 9, 11, 13, 17, 23, 32, 45, 52, 60, 72]
print(mylist)
for item in [60, 9, 10]:
  pos = binary_search(mylist, item, 0, len(mylist)-1)
  print("position of % 2d = %2d" %(item, pos))
```


#### 프로그램 3.4 재귀문 이진 탐색
```python
# 재귀문 이진 탐색
def rbinsearch(lst, item, left, right): # left, right : 인덱스 번호
  if left <= right: # while문 아니고 if문!!
    mid = (left+right)//2 # 중간 값 인덱스 번호
    if item == lst[mid]:
      return mid # 여기는 return문!
    elif item < lst[mid]:
      return rbinsearch(lst, item, left, mid-1)
    else: # item > lst(mid)
      return rbinsearch(lst, item, mid+1, right)
  return -1

mylist = [5, 8, 9, 11, 13, 17, 23, 32, 45, 52, 60, 72]
print(mylist)
for item in [60, 9, 10]:
  pos = rbinsearch(mylist, item, 0, len(mylist)-1)
  print("position of % 2d = %2d" %(item, pos))
```

## CHAPTER 3.3 피보나치 수열
#### 프로그램 3.5 피보나치 수열
```python
# 피보나치 수열
import time
def fibo1(n):
  global count1 # 재귀문에 계속 써야하니까 global로 선언
  count1 += 1
  if n <= 1:
    return n
  else:
    return fibo1(n-2)+fibo1(n-1)
  
def fibo2(n):
  global count2
  count2 += 2
  if n <= 1:
    return (0, n)
  else:
    (a, b) = fibo2(n-1)
    return (b, a+b)
  
for num_max in [10, 20, 30]:
  count1 = 0
  count2 = 0
  f = []
  print("fibo1", num_max)
  time1 = time.time() # 시간 측정
  for d in range(num_max):
    f.append(fibo1(d))
  time2 = time.time()
  print("time1 =", time2 - time1)
  print("recursion1 = ", count1, "for F%d"%num_max)
  print()

  time1=time.time()
  print("fibo2", num_max, "=", fibo2(num_max))
  time2=time.time()
  print("time2 =", time2-time1)
  print("recursion2 = ", count2)
  print(f)
  print()
```


## CHAPTER 3.4 하노이 타워
### 하노이 타워 알고리즘
```
세 개의 기둥 A,B,C가 있고 기둥 A에서 B로 n개의 원반을 옮긴다고 가정할 때,
1. 기둥 A에서 n-1개의 원반을 C로 옮긴다
2. 기둥 A에 남아있는 1개의 원반을 B로 옮긴다
3. 기둥 C에 있는 n-1개를 B로 옮긴다
```
#### 프로그램 3.6 하노이 타워
```python
# 하노이 타워
def htower(n, a, b): # n개의 원반을 a에서 b로 옮기기
  global count
  count += 1
  if n == 1:
    print("Disc %d, moved from pole(%d) to (%d)" %(n, a, b))
  else: 
    c = 6 - a - b # 비어있는 기둥
    htower(n-1, a, c) # 알고리즘 1번
    print("Disc %d, moved from pole(%d) to (%d)" %(n, a, b)) # 알고리즘 2번
    htower(n-1, c, b) # 알고리즘 3번

count = 0
n = int(input("Enter disc number:"))
htower(n, 1, 2)
print("# mover for %d discs : %d" %(n, count))
```

- 원반의 수가 n개일 때, (2^n)-1번의 이동이 필요하므로 시간 복잡도는 **O(2^n)** 이 된다


*** 
### + 순열
[a,b,c,d]의 순열
- (a, b, c, d), (a, b, d, c) ... => (a, Perm(b,c,d) => (b, Perm(c,d))....
- (b, Perm(a, c, d))
- (c, Perm(a, b, d))
- (d, Perm(a, b,c))

```python
# 순열 P (Permutation)
def perm(word, i, n): # i: 시작 인덱스, n: 끝 인덱스
  if i == n:
    print(word)
  else:
    for j in range(i, n+1):
      word[i], word[j] = word[j], word[i] # 순서 바꾸기
      perm(word, i+1, n)
      word[j], word[i] = word[i], word[j] # 원위치

word = ["l", "a", "n","g"]
perm(word, 0, 3)
```

<img width="230" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/e3735a15-da19-4aea-82fb-ae2dceb188bc">

## CHAPTER 3.5 미로 탈출
#### 프로그램 3.7 미로 탈출
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
    self.path = [] # 현재까지 경로 
    self.found = False

  def empty(self):
    return len(self.path) == 0 # path의 길이가 0이면 True = empty이다
  def add(self, row, col):
    self.path.append((row, col))
  def delete(self):
    return self.path.pop()
  def view(self):
    print(self.path)

  def rexplore(self, row, col):
    for x, y in [(0, -1), (1, 0), (0, 1), (-1, 0)]: # N, E, S, W
      next_row = row + x
      next_col = col + y
      if self.maze[next_row][next_col] == 'X': # 탈출 성공
        self.add(row, col)
        self.add(next_row, next_col)
        self.found = True
        return True
      elif self.maze[next_row][next_col] == 0 \
      and self.mark[next_row][next_col] == 0: # 미로 열려있음 && 아직 방문 X
        self.mark[next_row][next_col] = 1 # 방문 표시
        if (row, col) not in self.path: # 주의!!!!!
          self.add(row, col)
        result = self.rexplore(next_row, next_col)
        if result : return True
        else : self.delete()
    return False
  
m = Maze()
m.found = m.rexplore(1, 1)
if not m.found: print("탈출 경로 없음")
else: print("탈출 성공 :")
m.view()
```

1. self.path는 현재까지의 경로를 저장하는 리스트입니다.
2. if (row, col) not in self.path:는 현재 위치 (row, col)가 이미 경로에 포함되어 있는지 확인합니다.
3. 만약 (row, col)가 이미 경로에 있다면, 새로운 위치를 중복해서 추가하지 않기 위해 이 부분을 추가하였습니다.


self.mark 배열은 방문한 곳을 표시하는 용도로 사용되고, self.path 배열은 현재까지의 경로를 저장하는데 사용됩니다. self.path에 현재 위치를 추가하기 전에 중복 여부를 체크하여 중복 추가를 방지합니다. 
이 부분이 없다면, 재귀 호출 시 같은 위치를 여러 번 추가하게 되어 비효율적일 수 있습니다.

따라서 이 부분은 불필요한 중복 추가를 방지하기 위한 조건문으로, self.path에 이미 있는 위치일 경우 추가하지 않도록 하는 역할을 합니다.



- 이 프로그램의 시간 복잡도는 미로의 크기인 **O(row x col)** 에 비례한다


## CHAPTER 3.6 N-Queens 문제
#### 프로그램 3.8 N-Queens 문제
N개의 퀸을 nxn에 배치하기
```python
# N-Queens 문제
class NQueens:
  def __init__(self, size):
    self.size = size
    self.solutions = 0
    self.col = [-1]* size # 각 행의 몇 번 위치에 놓을 것인지 표시
    
  def solve(self):
    self.put_queen(0)
    print("Fount", self.solutions, "solutions")
  
  def put_queen(self, row):
    if row == self.size: # 마지막 행을 지나면 성공
      self.show_board()
      self.solutions += 1
    else:
      for col in range(self.size):
        if self.check_pos(row, col):
          self.col[row] = col
          self.put_queen(row + 1) # 다음행 가기
      
  def check_pos(self, rows, col): # 놓을 수 있는지 검사 함수
    for i in range(rows):
      if self.col[i] == col or self.col[i] - 1 == col - rows or self.col[i] + 1 == col + rows :
        return False
    return True
  
  def show_board(self):
    for row in range(self.size):
      line=""
      for column in range(self.size):
        if self.col[row] ==column: line+= 'Q '
        else: line += '+ '
      print(line)
    print("\n")

N = 6
q = NQueens(6)
q.solve()
```



<img width="545" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/34c5a4dc-04d1-409f-8af8-640b8626e693">


- n퀸즈 문제는 순열을 구하는 문제와 동일하다
- 따라서 시간 복잡도는 **O(N!)**








