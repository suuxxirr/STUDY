# section 3. 데이터와 C언어


## 3.1 데이터와 자료형

<img width="362" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/b46f9b30-4105-48cc-b6df-6cd3af6e651f">

## 3.2 변수와 상수 

``` C++
int angel = 1004; // 자료형, 변수, 리터럴 상수
```

- 리터럴 상수 : 문자 그대로의 의미를 갖고 값이 변할 수 없는 것

```C++
const int angel = 1004; // 한정자(제한자), 자료형, 기호적 상수, 리터럴 상수
```

## 3.3 scanf() 함수의 기본적인 사용법

```C++
#include <stdio.h>

int main() {

	int i = 0;

	scanf("%d", &i); // & : ampersand

	printf("Value is %d\n", i);

	return 0;
}
```

<img width="599" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/2b534345-2cb8-4417-807b-4048758977d6">

scanf를 호출할 때, 변수명 앞에 &를 붙여 **변수의 주소** 를 넘겨준다

## 3.4 간단한 입출력 프로그램 만들기

```C++
#include <stdio.h>

int main() {

	float won = 0;
	float dollar = 0.0f;

	printf("Input won\n");
	scanf("%f", &won);

	dollar = won * 0.00089f;

	printf("Dollar = %f", dollar);

	return 0;
}
```



서식 지정자 모음

<img width="454" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/030d18e6-0605-4e38-96cc-f10cf62e5dab">


## 3.5 정수와 실수

**정수** 
- 음의 정수, 0, 양의 정수
- 내부적으로 2진수

**실수**
- 2.0, 3.14, 0.123 ...
- 내부적으로 '부동소수점(floating point)'표현법 사용 (0.312E1)
- 내부적으로 2진수



**부호 있는 정수**

<img width="593" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/dce9cf21-5fc7-46fc-b1d8-327eaeb5424a">

**부동 소수점 수**

<img width="576" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/cfaa46e8-7c11-4f66-8168-42a997bf65d4">


<img width="518" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/365215d2-f616-49ea-82df-d67893dee2c5">

## 3.6 정수의 오버플로우

**sizeof()사용법**

```C++
#include <stdio.h>

int main() {

	unsigned int i = 0;
	printf("%u\n", sizeof(unsigned int)); 
	// 결과 값이 unsigned int로 출력되기 때문에 서식지정자 u사용
	printf("%u", sizeof(i));  
	// sizeof 연산자가 메모리 사이즈를 알려준다


	return 0;
}
```

**limits 헤더**

```C++
#include <stdio.h>
#include <limits.h> // 각 자료형이 가질 수 있는 가장 큰/작은 값을 알려줌

int main() {


	unsigned int i = 0b11111111111111111111111111111111;
	// 0b : 뒤의 숫자가 2진수라는 뜻
	// 이진수로 입력하고 싶을 때 사용
	// 모든 비트가 1로 꽉차있는 상태

	unsigned int u = UINT_MAX;
	printf("%u\n", i);
	printf("%u\n", u); // 동일한 값 출력

	return 0;
}
```

**오버플로우**

```C++
#include <stdio.h>
#include <limits.h> 
#include <stdlib.h>

int main() {


	unsigned int u_max = UINT_MAX + 1;

	// i to binary representation
	char buffer[33];
	_itoa(u_max, buffer, 2); // 변수가 담고 있는 숫자를 2진수로 변경

	// print decimal and binary
	printf("decimal: %u\n", u_max); // decimal : 0 출력
	printf("binary: %s\n", buffer); // binary : 0 출력

	return 0;
}
```


## 3.7 다양한 정수형들

<img width="555" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/25010c37-d560-4b22-b187-7fc02fd00b9b">

흐린 글자는 생략 가능

long (int)는 잘 사용되지 않는다
int가 2바이트로 쓰일 때 정수를 4바이트로 사용하려고 만들어짐


```C++
#include <stdio.h>

int main() {
	char c = 65;
	short s = 2;
	unsigned int ui = 3000000000U; /// 3'000'000'000U
	long l = 65537L;
	long long ll = 12345678908642LL; // 12'345'678'908'642ll

	printf("char = %hhd, %d, %c", c, c, c);
  // 65, 65, A 출력
  // %c는 문자로 출력해줌

	printf("short = %hhd, %hd, %d", s, s, s);
  // -56, 200, 200 출력
  // short를 char로 출력하려고 하면 오버플로우 발생

	printf("unsigned int = %u, %d\n", ui, ui);
  // 3000000000, -1294967296 출력
  // d로 출력하면 오버플로우 발생

	printf("long = %ld, %hd\n", l, l);
  // 65537, 1 출력
  // short로 출력하면 오버플로우 발생

	printf("long long = %lld, %ld\n", ll, ll);
  // 12345678908642, 1942899938 출력
  // long으로 출력하면 오버플로우 발생

	return 0;
}
```

## 3.8 8진수와 16진수

```c++
#include <stdio.h>

int main() {
	unsigned int decimal = 4294967295;
	unsigned int binary = 0b11111111111111111111111111111111; // 1 32개
	unsigned int oct = 037777777777; // 0 : 8진수 표현
	unsigned int hex = 0xffffffff; // 0x : 16진수 표현

	printf("%u\n", decimal);
	printf("%u\n", binary);
	printf("%u\n", oct);
	printf("%u\n", hex);

	printf("%o %x %#o %#x %#X", decimal, decimal, decimal, decimal, decimal);


	return 0;
}
```

<img width="406" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/5e2db214-689b-4882-8d9f-8afcc8afe803">

- %o : 8진수로 출력
- %x : 16진수로 출력
- %#o : #을 사이에 사용 시 prefix(0, 0x) 사용해서 출력
- %#x : f 소문자로 출력
- %#X : F 대문자로 출력



## 3.9 고정 너비 정수

고정 너비 정수는 프로그램의 이식성을 높일 때 사용한다

```c++
#include <stdio.h>
#include <inttypes.h>

int main() {

	int i;
	int32_t i32; // i32라는 변수는 32비트의 메모리만 사용
	int_least8_t i8; // 적어도 8비트 사용
	int_fast8_t f8; // fastest minimum
	intmax_t imax; // biggest signed integers
	uintmax_t uimax; // biggest unsigned integers

	i32 = 1004;

	printf("me32 = %d\n", i32);
	printf("me32 = %" "d" "\n", i32);
	printf("me32 = %" PRId32 "\n", i32); // 매크로로 교체

	return 0;
}
```


## 3.10 문자형

cahr 타입 변수에는 하나의 문자만 들어갈 수 있다

```c++
#include <stdio.h>

int main() {
	char c = 'A';
	char d = 65; // d = 'A'

	printf("%c %hhd\n", c, c);
	// A 65
	// 문자로 출력할 때는 %c

	printf("%c %hhd\n", d, d); 
	// A 65

	printf("%c \n", c+1);
	// B
}
```

## 3.11 부동소수점형

<img width="575" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/4abf70d3-2daf-444f-b8c7-b00eda0cd214">

컴퓨터에서 실수를 부동소수점 형태로 바꿔서 저장할 때 normalized sinificand 형태를 사용한다

<img width="599" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/c5878702-5e58-4757-9d87-a5388c8ac3ba">

- exponent
 exponent가 8비트 => 총 256가지 숫자 저장 가능 
 01111100 : 십진수로 124
 124-127 : signed int로 적은후 127을 빼주어 음수 표현



## 3.12 부동소수점형의 한계
```c++
#include <stdio.h>
#include <float.h>

int main() {

	/*roud - off errors(ex1)*/ 
	float a, b;
	a = 1.0e20f + 1.0f;
	b = a - 1.0320f;
	printf("%f\n", b); // 0.000000
	// 큰 숫자와 너무 작은 숫자를 더하면 작은 숫자가 사라져버림
	// 범위가 너무 다른 숫자끼리 연산하면 에러가 생길 수 있음

	/*/round - off errors(ex2)*/
	float c = 0.0f;
	for (int i = 0; i < 100; i++) {
		c += 0.01f;
	} 
	printf("%f\n", c); // 0.999999
	// 부동소수점 표현법에서는 0.01을 정확히 표현할 수 없음
	// 오차가 누적이 돼어 다른 값이 나옴


	/*overflow*/
	float max = 3.402823466e+38f; // float이 가질 수 있는 가장 큰 숫자
	printf("%f\n");
	max *= 100.0f;
	printf("%f\n"); // inf 출력 : (infinite) 너무커서 전부 출력 불가

	/*underflow*/
	float e = 1.401298464e-45f;
	printf("%e\n", e);
	e /= 2.0f; // 0 출력 
	// 숫자가 너무 작아서 날라감
	printf("%e\n", e);


	return 0;
}
```

## 3.13 불리언형
true : 1
false : 0









