# section 5. 배열과 컬렉션 프레임워크
## section 5.1 배열 1편
#### 배열이란?
- 참조 타입
- 같은 타입의 변수가 여러개 필요할 때 사용
- 배열은 **기본형 배열**과 **참조형 배열**로 나뉜다

#### 기본형 배열
- boolean, byte, short, char, int, long, float, double 타입의 변수를 여러개 선언할 필요가 있을 때 사용
- 기본형 배열 선언

```
기본형타입[] 변수명;
기본형타입 변수명[];
```
<img width="370" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9e6e4df8-54e2-4ca9-bdb9-8795d96538fd">
- 예제1

```java
int[] array1;
int array2[];  //array1, array2, array3은 배열을 가리킬 수 있는 변수
int array3[];

array1 = new int[5];
array2 = new int[5];
array3 = new int[0]; // 정수를 아무것도 가질 수 없는 배열 
```
- 예제2

```java
int[] array1, array2; // array1, array2 모두 정수 배열 >> 둘 다 배열
int array3[], array4; // int type의 array3 배열, int type의 array4 !array3만 배열!
```
- 배열 선언과 동시에 선언

```java
기본형타입[] 변수명 = new 기본형타입[배열의크기];
변수명[index값] = 값;
기본형타입[] 변수명 = new 기본형타입[]{ 값1, 값2, ....};
기본형타입[] 변수명 = {값1, 값2, 값3....};
```

#### 참조형 배열
- 배열의 타입이 기본형이 아닌 경우
- 배열 변수가 참조하는 배열의 공간이 값을 저장하는 것이 아니라 값을 참조한다는 것을 의미한다

```java
public class ItemForArray {
	private int price;
	private String name;
	
	public ItemForArray(int price, String name) {
		this.price = price;
		this.name = name;
	}
	
	public int getPrice() {
		return price;
	}
	
	public String getName() {
		return name;
	}
}
```
```java
public class Array03 {
	public static void main(String[] args) {
		ItemForArray[] array1;
		ItemForArray array2[];
		
		array1 = new ItemForArray[5];
		array2 = new ItemForArray[5];
		
		array1[0] = new ItemForArray(500, "item01"); // 배열이 ItemForArray 인스턴스 참조
		array1[1] = new ItemForArray(500, "item02");
	}
}
```

#### 배열의 길이 구하기
- 배열은 **length 필드**를 가진다

```java
double[] array1 = new double[5];
double[] array2 = {1.5, 2.4, 3.5};
double[] array3;
double[] array4 = null;

System.out.println(array1.length); // 5
System.out.println(array2.legth); // 3
System.out.println(array3.length); // error (초기화안해줘서)
System.out.println(array4.length); // error
```

## 이차원 배열
```
타입[][] 변수명 = new 타입 [행의수] [열의수];
변수명 [행인덱스] [열인덱스] = 값;
```
```java
int[][] array = new int[2][3];
array[0][0] = 0;
array[0][1] = 1;
array[0][2] = 2;
array[1][0] = 3;
array[1][1] = 4;
array[1][2] = 5;
```
<img width="412" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/db7bea0a-2e2f-4697-8259-e3b4cc7fd0ce">

- 배열 선언과 동시에 초기화

```java
.
```













