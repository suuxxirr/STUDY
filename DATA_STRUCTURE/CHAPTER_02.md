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

#### 프로그램 2.6 주사위 던지

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




















