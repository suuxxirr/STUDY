# CHAPTER 02 파이썬 자료구조
## CHAPTER 2.1 파이썬 특징
- 인터프리터형 언어 (vs 컴파일러형 언어)
  : 컴파일 과정 없이 문장 단위로 빠르게 실행과 테스트 가능
- 객체 지향 언어
- 동적 타이핑 언어
  : 자료형 선언 x
- 리스트, 집합, 딕셔너리 등 시퀀스 자료형과 군집 자료형 지원 기능 우수
- 파이썬 변수는 값에 대한 참조(reference to literal)
- 동적 할당
  : 리스트에 원소를 추가할 때마다 크기 변화




## CHAPTER 2.2 파이썬 자료형
#### 프로그램 2.2 팩토리얼
```python
# 팩토리얼 계산하기
def fact(n):
    total = 1
    for j in range(2, n+1):
        total *= j
    return total
    
    
i = 20   
for n in range(2, i+1):
    print(n, "!=", fact(n))

```


## CHAPTER 2.3 파이썬의 변수
파이썬에서 변수는 대상 값(리터럴)에 대한 참조(reference)이다
(리터럴 : 변수가 참조하는 값)


파이썬 자료형은 **불변 자료형**과 **가변 자료형**으로 나뉜다 
### 불변 자료형
불변 자료형에는 정수, 실수, 문자열이 있으며 원래 값의 변경이 불가능하다
- int, float, string
- 리터럴 변경 불가능
- 값 변경시 새로운 리터럴 참조
- 매개변수 전달시 값 변경 **불가능**


#### 프로그램 2.3 불변 자료형
```python
# 불변 자료형
a = 2
b = a
print("a", a, id(a)) # id(): 참조하는 대상의 주소
print("b", b, id(b)) 
# 주소가 같다

b = b + 1
print("a", a, id(a))
print("b", b, id(b)) # b는 새로 생성된 리터럴을 가리키게 됨
# 주소 다르다 
```

<img width="113" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ab6d9148-bd62-43d9-8849-22fda9d47c80">




### 가변 자료형
리스트, 딕셔너리와 같은 군집 자료형은 원소를 수정할 수 있는 가변 자료형이다

- List, dictionary
- 리스트, 사전의 원소 값 변경/추가 가능
- 리스트는 각 원소 값에 대한 참조들의 모음
- 매개변수 전달시 인수 값이 변경될 수 **있음**


#### 프로그램 2.4 가변 자료형
```python
# 가변 자료형
A = [2, 3, 4]
B = A
print("A", A, id(A))
print("B", B, id(B))

B.append(5)
print("A", A, id(A))
print("B", B, id(B))
# 리스트에 원소를 추가한 후에도 리스트 주소에 변화가 없다
```


<img width="174" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/b9025b34-a2b9-498f-a115-4ce249aa6ef4">



### 파이썬 함수에 매개변수 전달

리스트와 딕셔너리는 함수에서 참조에 의한 호출을 통해 수정 가능
그러나 불변 자료형에 대해서는 이 방법을 지원하지 않기 때문에 return, global, 객체의 멤버 변수를 이용해야 한다

(참고)

https://aalphaca.tistory.com/4

https://velog.io/@yun9yu/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%80-Call-by-reference-Call-by-value


#### 프로그램 2.5 불변 자료형 변수를 함수에 전달
```python
# 매개변수 전달
def f(a):
    a = 3
    print(a)
    return a

b = 2
f(b) # 3
print(b) # 2
b = f(b) # 3
print(b) # 3
```

#### 프로그램 2.6 주사위 던지기 

```python
# 주사위 던지기 (함수에 리스트 전달)
# 주사위를 던져서 각 눈이 나오는 비율을 계산

import random
def throw_dice(count, n):
    for i in range(n):
        eye = random.randint(1, 6)
        count[eye-1] += 1

def calc_ratio(count):
    total = sum(count)
    ratio = []
    for i in range(6):
        ratio.append(count[i]/total)
    return ratio

def print_percent(num):
    print("[", end = '')
    for i in num:
        print("%4.2f" % i, end = ' ')
    print("]")
    

dice = [0]*6
for n in [10, 100, 1000]:
    print("Times = ", n)
    throw_dice(dice, n)
    print(dice)
    ratio = calc_ratio(dice)
    print_percent(ratio)
    print()
```




### 별명(alias)
별명 : 두 변수가 하나의 객체를 동시에 참조하여 공유하는 것

#### 프로그램 2.7 별명
```python
# 별명
A = [[2, 3, 4], 5]
B = A # 같은 객체 참조
B[0][1] = 'f'
print(A, id(A))
print(B, id(B)) # 주소 같다 
```


### 얕은 복사
얕은 복사 : 변수는 복사하여 새로 생성하지만, 참조하는 리터럴은 공유하는 복사 방법
copy모듈의 copy()함수를 사용하여 실행한다

#### 프로그램 2.8 얕은 복사 

```python
# 얕은 복사
import copy
A = [[2,3,4], 5]
B = copy.copy(A) # B = list(A)
B[0][1]='f'
B[1] = 7
print(A) # [[2, 'f', 4], 5]
print(B) # [[2, 'f', 4], 7]
```


<img width="574" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/1128d076-aac5-4a55-afdd-ca7b9aab4eff">


### 깊은 복사 
깊은 복사 : 변수와 대상 객체를 모두 복제하여 생성

#### 프로그램 2.9 깊은 복사 
```python
import copy
A = [[2, 3, 4], 5]
B = copy.deepcopy(A) # 깊은 복사
B[0][1]= 'f'
B[1] = 7
print(A) # [[2, 3, 4], 5]
print(B) # [[2, f, 4], 7]
```



## CHAPTER 2.4 리스트

- 정적배열(array) : c언어 등에서 사용
  - 원소 자료형 동일
  - 선언한 이후 배열 크기 변경 불가능
  - 각 원소 공간에 값이 직접 저장
  - 인덱스로 원소 값 접근
 
- 리스트 : 파이썬에서 사용
    - 리터럴에 대한 참조 변수
    - 참조 대상의 자료형 동일할 필요 x
    - 리스트 크기가 원소의 추가 또는 삭제에 의해 동적으로 변경
 
### 리스트의 생성
리스트는 다음의 세가지 방법으로 생성 가능
1. 리스트 복합 표현
2. range()와 list()함수 사용
3. 빈 리스트 선언 후 원소 추가


#### 프로그램 2.10 리스트 생성
```python
 # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
num1 = [i for i in range(10)]
num2 = list(range(10))
num3 = []
for i in range(10):
    num3 = num3 + [i] # num3.append(i)
```

### 예제 : 볼링 점수 계산

#### 프로그램 2.12 볼링 게임 점수 계산
```python
# 볼링게임 점수 계산
score1 = [(8,0), (4,3), (8,2), (4,6), (2,6),(10,0),(9,0),(10,0),(8,2),(10,0),(10,10)]
score2 = [(10,0), (10,0), (10,0),(10,0),(10,0),(10,0),(10,0),(10,0),(10,0),(10,0),(10,10)]

score_list = [score1, score2]
for score in score_list:
    i = total = 0
    frame = []
    for first, second in score:
        f_total = first + second
        next_first, next_second = score[i+1]
        if first == 10: # strike
            result = 'STRIKE'
            f_total += next_first + next_second
            
            # 1~9 프레임에서 더블(=두 번 연속 스트라이크)을 친 경우
            if i != 9 and next_first == 10:
                next_next_first, next_next_second = score[i+2]
                f_total += next_next_first # 중요
        elif (first + second) == 10: # spare
            f_total += next_first
            result = 'SPARE'
        else : result = 'NONE'
        
        total += f_total
        i += 1
        frame.append((f_total,result))
        if i == 10: break
    
    print(frame)
    print("Total =", total)
    print()
            
```
## CHAPTER 2.5 집합과 딕셔너리
### 집합
```
집합 선언 방식
s1 = set([1, 2, 3, 3, 2])
s2 = {2, 3, 4}
```
### 딕셔너리
- items() : 딕셔너리 각 항목을 튜플(key, value)로 반환
- keys() : 딕셔너리 각 항목의 키만을 추출하여 리스트로 반환
- values() : 딕셔너리 각 항목의 값만을 추출하여 리스트로 반환
- 
### 희소 행렬 
```python
# 희소 행렬 리스트에 담기 
class SparseMatrix:
  def __init__(self):
    self.matrix = [[15, 0, 0, 22, 0, 15], \
                   [0, 11, 3, 0, 0, 0],\
                   [0, 0, 0, -6, 0, 0],\
                   [0, 0, 0, 0, 0, 0],\
                   [91, 0, 0, 0, 0, 0],\
                   [0, 0, 28, 0, 0, 0]]
    self.sparse_list=[]

  def toTuple(self):
    row = count = 0
    for rows in self.matrix:
      col = 0
      for values in rows:
        if values != 0:
          count += 1
          self.sparse_list.append((row, col, values))
        col += 1
      row += 1 
    height = len(self.matrix)
    width = len(self.matrix[0])
    self.sparse_list.insert(0, (height, width, count))

s = SparseMatrix()
s.toTuple()
print(s.sparse_list)
```


## 연습문제 
### 01
```python
list = [21, 7, 40, 29, 11, 5, 90, 78, 64, 15, 88]
max = -100
min = 100

for i in range(len(list)):
    if list[i] > max : max = list[i]
    if list[i] < min : min = list[i]
    
tuple = (max, min)
print(tuple)
```
### 02
```python
def multiple():
    list = []
    for i in range(1, n):
        if i % m == 0:
            list.append(i)
    return list


n = 10
m = 3
print(multiple())
```

### 03
```python
def make_fraction():
    num = float(input("숫자 입력 : "))
    i = 0
    while not (num.is_integer()):
        num *= 10
        i += 1
    den = 10**i

    # 약분
    a = num
    b = den
    while a % b != 0:
        a, b = b, a % b
    gcd =  b # 최대 공약수
    print(f'{int(num/gcd)}/{int(den/gcd)}')
    
    
make_fraction()

```
### 04
```python
def check_palindrome(s):
  ls = list(s)
  if len(s) % 2 == 0: # 문자열 길이 짝수
    length = int(len(s) / 2)
    s1 = ls[:length]
    s2 = ls[length:]
    print(s1 == s2[::-1])
  else : # 문자열 길이 홀수 
    length = int((len(s) + 1) / 2)
    s1 = ls[:length - 1] 
    s2 = ls[length:] 
    print(s1 == s2[::-1])
check_palindrome('abcnncba') # True
check_palindrome('abcncba') # True
check_palindrome('hello') # False
```

### 05
```python
list = [] 
for i in range(1, 44, 3):
  list.append(i)

print(list)
```

### 06
```python
list = []
n = 0
i = 1
while i < 106:
  i += n 
  list.append(i)
  n += 1
print(list)
```

### 07
```python
scores = [[75, 90, 85], [60, 100, 75], [90, 70, 80]]
for i in range(len(scores)):
  for j in range(len(scores[i])):
    if j == len(scores[i]) - 1:
            print(scores[i][j], end='')
    else:
          print(scores[i][j], end=', ')
  print()
```

### 08
```python
# polynominal
def padd(a, b, d): # a + b = d
  while a and b : # a와 b가 비어있지 않는 동안 
    coef1, exp1 = a[0]
    coef2, exp2 = b[0]
    if exp1 > exp2 : 
      d.append(a.pop(0))
    elif exp1 < exp2:
      d.append(b.pop(0))
    else:
      if (coef1 + coef2): # 0이 아닐 때만 실행!
        d.append((coef1+coef2, exp1))
      a.pop(0)
      b.pop(0)
  for coef, exp in a: # 남은 거 처리
    d.append((coef, exp))
  for coef, exp in b:
    d.append((coef, exp))


a = [(5, 12), (- 6, 8), (13, 3)]
b = [(10, 15), (4, 8), (9, 0)]
d = [ ]


print("a =", a)
print("b =", b)
padd(a, b, d)
print("d =", d)
```

### 09
```python
sentence = "I know what you like boy\
You’re my chemical hype boy\
Open my eyes to see old days gone like a dream\
Hype boy, all I wanna, hype boy, gonna tell ya"

sentence = sentence.lower() # 소문자로 변환
words = sentence.split() # 단어 단위로 분리 (리스트)
dic = {}
for word in words:
  if word not in dic:
    dic[word] = 0
  dic[word] += 1
print("# of different words = ", len(dic))


# 단어의 빈도를 내림차순으로 정렬하여 출력
sorted_words = sorted(dic.items(), key=lambda x: x[1], reverse=True)
for word, frequency in sorted_words:
    print(f"{word}: {frequency} times")
```

- 딕셔너리에서 .keys()와 .values()를 이용하면 dict_keys,dict_values 형태로 값을 출력하기 때문에 리스트로 추출하고 싶다면 
`list()`를 이용해 변환해주어야 한다 


#### sorted()

sorted() 함수는 첫 번째 매개 변수로 들어온 이터러블한  데이터를 새로운 정렬된 리스트로 만들어서 반환해준다
- 첫 번째 매개변수로 들어올 "정렬할 데이터"는 iterable한 데이터이어야 한다
- key 옵션
  - 어떤 것을 기준으로 정렬할 것인가에 대한 기준. sorted(~~, key=어쩌구)로 사용
- reverse 옵션
  - 오름차순/내림차순 결정



### 10
```python
class SparseMatrix:
  def __init__(self): 
    self.matrix = [[0, 3, 0, 2],\
                   [8, 0, 4, 0], # 인덱스 [0] : (행, 열, 0이 아닌 원소 수)
                   [0, 0, 0, 5] # [(3, 4, 5), (0, 1, 3), (0, 3, 2), (1, 0, 8), (1, 2, 4), (2, 3, 5)]
                  ]
  
    self.sparse_list=[]
# 희소 행렬 리스트에 담기 
  def toTuple(self):
    row = count = 0
    for rows in self.matrix:
      col = 0
      for values in rows:
        if values != 0:
          count += 1
          self.sparse_list.append((row, col, values))
        col += 1
      row += 1 
    height = len(self.matrix)
    width = len(self.matrix[0])
    self.sparse_list.insert(0, (height, width, count))

# 리스트 다시 행렬 형식으로 출력
  def toMatrix(self, ls):
    max_row, max_col, n = ls[0]
    item_ls = ls[1:]  # [(0, 1, 3), (0, 3, 2), (1, 0, 8), (1, 2, 4), (2, 3, 5)]
    # max_row = 3, max_col = 4
    for row in range(max_row): 
      for col in range(max_col):
        i = 0
        if i >= max_row : break
        flag = False
        while not flag:
          if (item_ls[i][0] == row and item_ls[i][1] == col):
            print(item_ls[i][2],end = ' ')
            flag = True
          elif i == (len(item_ls) -1):
            print('0',end = ' ')
            flag = True
          else:
            i += 1
      print()

s = SparseMatrix()
s.toTuple()
print(s.sparse_list)
s.toMatrix(s.sparse_list)
```



























