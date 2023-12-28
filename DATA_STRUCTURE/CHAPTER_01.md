# CHAPTER 01 자료구조 개요
## CHAPTRER 1.1 소프트웨어와 자료구조
### 자료구조
자료구조 : 프로그램을 통해 대량의 데이터를 효과적으로 저장하고 처리하기 위한 방법론
- 선형 자료구조 : 배열, 스택, 큐, 리스트
- 비선형 자료구조 : 트리, 그래프


## CHAPTER 1.2 소프트웨어 개발 주기
1. 요구사항
2. 분석
3. 설계
4. 구현
5. 검증



## CHAPTER 1.3 알고리즘의 정의
**알고리즘**은 주어진 문제 해결에 필요한 유한한 명령어들의 집합으로 정의하며, 다음의 조건을 만족해야 한다

1. 입력 : 0개 이상의 입력이 있어야 한다 (명시적인 외부 입력이 반드시 필요한 것은 아님)
2. 출력 : 1개 이상의 출력이 있어야 한다
3. 명확성 : 모호하지 않은 명령어로 표현되어야 한다
4. 유한성 : 유한한 수의 명령어로 이루어져야 한다
5. 유효성 : 중복되고 불필요한 명령어 없이 효율적으로 작성되어야 한다


#### 프로그램 1.1
```python
def fibo(fb):
    a = 0
    b = 1
    while True:
        a, b = b, a + b
        if b > n : break 
        fb.append(b)
        

fb = [0, 1]
n = 10000
fibo(fb)
print(fb)
```

#### 프로그램 1.2
```python
# 소수 찾기
def find_prime(num):
    for i in range(2, num+1):
        prime = 1 #True
        for j in range(2, i):
            if i % j == 0 : 
                prime = 0
                break
        if prime:
            prime_list.append(i)
            
max_num = 1000
prime_list = []
find_prime(max_num)

# 출력
for i in range(2, max_num):
    if i in prime_list: print("%3d" %i, end = ' ')
    else : 
        print('.', end = ' ')
    if i % 50 == 0 : print()
```


## CHAPTER 1.4 추상 자료형
**추상 자료형(abstract data type: ADT)** : 구현하고자 하는 구조에 대해 구현 방법은 명시하지 않고 자료구조의 특성들과 어떤 Operations들이 있는지를 설명하는 자료구조의 한가지 형태. 대표적인 예로 스택과 큐가 있다


#### 프로그램 1.3 
```python
# 1.3 분수 연산과 출력하기
class Fraction:
    def __init__(self, up, down):
        self.num = up
        self.den = down
        
    def __str__(self):
        return str(self.num) + "/" + str(self.den)
    
    def __add__(self, other):
        new_num = self.num * other.den + self.den*other.num
        new_den = self.den*other.den
        common = self.gcd(new_num, new_den)
        return Fraction(new_num//common, new_den//common)
    
    def multiply(self, other):
        new_num = self.num * other.num
        new_den = self.den * other.den
        common = self.gcd(new_num, new_den)
        return Fraction(new_num//common, new_den//common)
        
    def gcd(self, a, b):
        while a % b != 0:
            a, b = b, a % b
        return b
    
num1 = Fraction(1, 4)
num2 = Fraction(1, 2)
num3 = num1 + num2
print(num1, "+", num2, "=", num3)
num4 = num1.multiply(num2)
print(num1, "*", num2, "=", num4)
        
```

```
최대 공약수 공식(수학 공식 같은 거니까 그냥 외우기)
gcd(x,y) = gcd(y, x%y if y > 0
gcd(x,0) = x
```



## CHAPTER 1.5 프로그램 성능 평가
- 공간 복잡도 : 프로그램의 수행을 완료하는데 필요한 메모리의 양(memory space)
- 시간 복잡도 : 프로그램 수행을 완료되는데 걸리는 시간(computation time)



### 공간 복잡도(space complexity)
알고리즘 실행에 필요한 메모리의 양
알고리즘이 사용하는 메모리 공간은 다시 
1. **고정 요구 공간**
2. **가변 요구 공간**

으로 구분한다

- 고정 요구 공간(fixed memory space)
  - 프로그램이 실행 중에 크기의 변동이 없는 메모리 공간
  - ex) 프로그램 명령어, 고정 크기 변수, 상수 등
- 가변 요구 공간(variable memory space)
  - 프로그램 실행 중에 요청되어 그 크기를 예측하기 어려운 경우
  - 1. **배열 전달(array passing)** 2. **재귀 호출(recursion)**
   
### 시간 복잡도(time complexity)
프로그램의 실행을 완료하는데 필요한 시간
컴파일 시간과 실행시간으로 구분한다
- T = compile time + execution time

시간 복잡도 계산을 위해 프로그램의 물리적인 실행 시간을 측정하는 방법은 환경에 따라 매번 달라지는 문제가 발생하기 때문에, 실제 실행시간보다는 **실행하는 명령문의 개수**를 세어 시간 복잡도로 사용한다

#### 명령문 측정표
<img width="527" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/0022b6ce-79a0-425d-885f-eb1e164139af">


<img width="511" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9649e078-12de-4142-aecc-f01403131aa8">

<img width="505" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/fe55400f-e28a-47c7-9ce6-fbb61e63bcb0">

































