# section 4. 문자열과 형식 맞춘 입출력

## 4.1 문자열 입출력하기
```C++
#include <stdio.h>

int main() {
	char fruit_name[40]; // 1바이트 저장공간 40개 확보

	printf("what is your favorite fruit? : ");

	scanf("%s", fruit_name);
	// 문자열 출력 시에는 & 사용 X
	// 배열명 자체가 주소이기 때문에

	printf("You like %s!\n", fruit_name);

	return 0;
}
```

## 4.2 sizeof 연산자

sizeof 연산자 : 자료형이 차지하는 메모리의 크기를 알려준다

```c++
#include <stdio.h>

int main() {
	/* 1. sizeof basic types*/

	int a = 0;
	unsigned int int_size1 = sizeof a;
	unsigned int int_size2 = sizeof (int);
	unsigned int int_size3 = sizeof(a);

	size_t int_size4 = sizeof(a);
	size_t float_size = sizeof(float);

	printf("Size of int type is %u bytes.\n", int_size1); // 4
	printf("Size of int type is %zu bytes.\n", int_size4); // 4
	// %zu : size_t에 대응하는 형식지정자
	printf("Size of int type is %zu bytes.\n", float_size); // 4

	/* size of arrays*/
	int int_arr[30];
	printf("Size of array = %zu bytes \n", sizeof(int_arr)); // 120 (4*30)
	return 0;

	/* size of charater array*/

	char c = 'a';
	char string[10]; // maximallly 0 character + '/0' (null character)

	size_t char_size = sizeof(char);
	size_t str_size = sizeof(string);

	printf("Size of char type is %zu bytes.\n", char_size); // 1
	printf("Size of string type is %zu bytes. \n", str_size); // 10
}
```

## 4.3 문자열이 메모리에 저장되는 구조

```c++
#include <stdio.h>

int main() {
	/* 숫자 배열*/
	int a = 1;
	int int_arr[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

	printf("%i %i %i\n", int_arr[0], int_arr[1], int_arr[9]);

	/* 문자 배열*/
	char c = 'a';
	char str1[10] = "Hello";
	char str2[10] = { 'H', 'i' };

	printf("%c\n", c); // a
	printf("%s\n", str1); // Hello
	printf("%s\n", str2); // Hi

}
```

## 4.4 strlen() 함수

```c++
#include <stdio.h>
#include <string.h> // strlen() 함수 사용 시 필요

int main() {
	char str1[100] = "Hello";
	char str2[] = "Hello"; // 빈칸으로 해놓으면 알아서 맞춰 지정 => 6칸 할당
	char str3[100] = "\0";
	char str4[100] = "\n";

	printf("%zu %zu\n", sizeof(str1), strlen(str1)); // 100 5
	printf("%zu %zu\n", sizeof(str2), strlen(str2)); // 6 5
	printf("%zu %zu\n", sizeof(str3), strlen(str3)); // 100 0
	printf("%zu %zu\n", sizeof(str4), strlen(str4)); // 100 1


	return 0;
}
```


## 4.5 기호적 상수와 전처리기
```c++
#include <stdio.h>;
#define PI 3.141592f // 기호적 상수 
int main() {
	// const float pi = 3.141592f;
	// 이렇게 const를 사용해서 정의해도 된다 (c++에서 완전 권장)

	float radius, area;
	printf("Input radius\n");
	scanf("%f", &radius);
	area = PI * radius * radius; 

	printf("Area is %f\n", area);


	return 0;
}
```




