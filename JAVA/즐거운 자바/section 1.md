# section 1. 자바 기본 문법
## section 1.1 변수와 리터럴

> 자바의 타입에는 **기본형 타입**과 **레퍼런스 타입** 2가지 종류 존재


기본형 타입
- 첫 번째 글자가 모두 소문자
- ex) int, long, short, double, float

레퍼런스 타입
- 기본형이 아닌 타입은 모두 레퍼런스 타입
- 대문자로 시작
- ex) class, interface ...

>기본형 타입의 변수는 모두 메모리를 확보하고 값을 가진다    
> But 기본형인 아닌 타입(=참조형 타입)은 값을 참조한다


#### 변수명 규칙
- 하나 이상의 글자
- 첫 번째 글자는 문자이거나 $, _
- 길이 제한 x
- 키워드는 변수명으로 사용 x

#### 정수 타입 변수 선언
> 정수 타입 변수를 선언 후 아무 값도 대입하지 않으면 0 값을 가지게 된다


## section 1.2 코드는 한 줄씩 차례대로 실행된다
자바 프로그램은 위에서부터 아래로 코드를 한 줄씩 실행한다

## section 1.3 논리형 타입과 논리 연산자
#### 논리형 타입 boolean
- boolean 타입은 true, fale 2가지 값 중 하나를 가진다
- 초기화하지 않으면 기본적으로 **false**값을 가진다
- 논리 연산의 결과를 저장할 때 사용
- 메모리에 1바이트 사용 (1비트로도 참/거짓 표현 가능하지만 컴퓨터는 자료를 표현하는 최소 단위가 1바이트이기 때문에)

> 메소드 안에 선언된 지역변수는 반드시 초기화를 하고 사용해야 한다
> > 그렇지 않으면 오류 발생     
> 클래스 안에 선언된 변수(=필드)는 초기화를 하지 않아도 사용 가능하다



#### 논리 연산자 ^
- exclusive-or (XOR)
- 2개의 식의 논리 값이 서로 다를 경우

#### 부정 연산자 !
- 논리형 값을 부정
- ```boolean a = !(10>5);``` => a에 false 저장


## section 1.4 정수와 실수 그리고 산술 연산자

#### 정수형 타입
- 기본형은 int

#### 실수형 타입
- float, double
- 기본형 double

#### Integer 클래스
- MIN_VALUE, MAX_VALUE 필드를 가진다
- ```Integer.MAX_VALUE```: int가 갖는 최댓값
- ```Integer.MIN_VALUE```: int가 갖는 최솟값


#### 오버플로우
- 계산 결과가 최댓값을 넘거나, 최솟값보다 작을 경우 음수는 양수로, 양수는 음수로 바뀌는 문제가 발생. 이를 오버플로우라고 한


## section 1.5 타입의 변환
double형 타입은 정수값이 잘 대입된다
#### 묵시적 타입 변환(자동 타입 변환, implicit conversion)
```java
double d1 = 50;
double d2 = 500L;
```
int형 리터럴 50, long형 리터럴 500이 모두 d1, d2에 저장된다


But int형에 실수를 대입하면 오류 발생
실수는 정수를 포함하지만, 정수는 실수를 포함할 수 없기 때문에 아래 코드는 컴파일 오류 발생
```
int i1 = 50.0;
int i2 = 35.4f;
```

#### 명시적 타입 변환(강제 타입 변환, explicit conversion)
- 변환하고자 하는 값 앞에 (타입) 붙이기
```java
int i1 = (int)50.0;
int i2 = (int)25.6f;
```

#### 크기가 큰 타입은 작은 타입을 저장 가능
- long 타입의 변수는 byte, short, int 타입 값 저장 가능
- int 타입의 변수는 byte, short 타입의 값 저장 가능
- short 타입의 변수는 byte 타입의 값 저장 가능

```java
short s = 5;
int i = s;
long x = i;
```
> 형변환시 크기가 큰 타입을 작은 타입에 저장할 때는 오버플로우 조심!


## section 1.6 문자(char)타입
#### 문자타입
- 문자는 작은 따옴표로 묶인 문자 하나
- 문자는 2바이트 크기를 가지며 **유니코드 값**을 가진다

> 문자 타입은 정수 타입이기도 하다
> > 문자 타입은 0~55535까지 저장할 수 있는 정수 타입이기도 하다
```java
public class chartype {

	public static void main(String[] args) {
		char c1 = 'a';
		
		System.out.println((int)c1); // 97 출력
		
		char c2 = (char) 97;
		System.out.println(c2); // a 출력
	}

}
```
## section 1.7 비트 연산자
#### 바이트
- 1바이트는 00000000부터 11111111까지 값을 표현 가능
- 1바이트는 정수로 표현하면 0부터 254까지 표현 가능 (부호 비트 없을 때)
- 1바이트를 16진수로 표현하면 00~FF까지 표현 가능


#### 비트 연산자
비트 연산자는 논리 연산자와 비슷하지만, 비트 단위로 논리 연산을 할 때 사용하는 연산
즉, 바이트를 구성하고 있는 비트를 연산하는 연산
- &, |, ^, ~
- ```<<``` : 좌측 시프트    
  ```>>``` : 우측 시프트    
  ```>>>``` : 우측 양수화 시프트

#### >> 와 << 와 >>>
- ```<<``` : 명시된 수만큼 비트들을 전부 왼쪽으로 이동
- ```>>``` : **부호를 유지**하면서 지정한 수만큼 비트를 전부 오른쪽으로 이동
- 정수형 타입을 비트로 표현했을 때, 맨 좌측의 비트는 부호화 비트      
  맨 좌측의 비트가  0이면 음수, 1이면 양수

```java
public class bitoperatorexam1 {

	public static void main(String[] args) {
		int a = 4; // 00000100 (맨 오른쪽 1바이트만 표현)
		int b = a >> 1; // 0000 0010
		System.out.println(b); // 2 출력
		
		int c = 4; // 0000 0100
		int d = c << 1; // 0000 1000
		System.out.println(d); // 8 출력 

	}

}
```

- ```>>>``` : 지정한 수만큼 비트를 전부 오른쪽으로 이동시키며, 새로운 비트는 전부 0이 된다
- ```>>>```는 그 결과가 무조건 양수
- 10000000 >>> 2 를 하면 00100000


## section 1.9 조건문 if와 삼항 연산자
#### if 조건문
- if 문장에 중괄호가 없을 경우 if 문장 다음 문장만 조건에 만족할 경우 실행 
#### 삼항 연산자
- 자바에는 항이 3개인 연산자 하나 존재
- ```조건식 ? 반환값 1 : 반환값 2```      
    : 조건식이 참일 경우 반환값 1이 사용되고, 거짓일 경우 반환값 2가 사용된다

## section 1.10 조건문 switch
#### switch 사용법
- switch 블록 안에 여러개의 case가 올 수 있다
- switch 블록 안에 하나의 default가 올 수 있다
- break문은 생략 가능

``` java
switch (변수) {
  case 값1 :
    변수가 값1일 때 실행
    break;
  case 값2 :
    변수가 값2일 때 실행
    break;
  ...
default:
    변수의 값이 어떤 case에도 해당되지 않을 경우 실행
}
```

## section 1.11 반복문 while
#### while문과 후위 증감식
- 변수 뒤에 후위 증가식이 붙을 경우 변수가 사용된 이후에 값이 증가된다
- i와 10이 비교를 한 후 i가 1 증가한다
```java
int i = 0;
		while(i++ < 10) {
			System.out.println(i); //1~10까지 출력
		}
```
#### while문과 continue
- while문에서 continue를 만나면, continue 이하 문장을 실행하지 않고 반복한다 


## section 1.13 반복문 for
cf) 문자열과 더해지면 문자열이 된다
- 문자열  + 정수
  "hello" + 1 => "hello1"
- 문자열 + 불린
  "hello" + true => "hellotrue"
- 문자열 + 실수
  "hello" + 50.4 => "hello50.4"

## section 1.14 반복문과 label
#### break와 continue문의 한계
- break는 현재 반복문을 빠져나가는데 사용
- continue는 continue문 아래 부분을 실행하지 않고 다시 반복
- 그렇다면 중첩 반복문을 한번에 빠져나가려면, continue 이하를 실행하지 않고 한번에 중첩 반복문을 반복하려면?     
=> **이럴때 label 사용**

```java
public class labelexam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		outter:
		for (int i = 0; i < 3; i++) {
			for(int k = 0; k < 3; k++) {
				if (i == 0 && k == 2) 
					break outter;
				System.out.println(i+ "," + k);
				
			}
		}
	}

}
```
i가 0, k가 2일 때 중첩 반복문 빠져나간다







