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


- 이차원 배열 선언과 동시에 초기화

```java
int[][] array = {{0, 1, 2}, {3, 4, 5}}
```

#### 이차원 가변 배열

```java
타입[][] 변수명 = new 타입 [행의수][];
변수명[행의인덱스] = new 타입[열의수];
```

```java
int[][] array = new int[2][];
array[0] = new int[2];
array[1] = new int[3];
```



- 이차원 가변 배열 선언과 동시에 초기화

```java
int[][] array = {{0,1},{2,3,4}};
```

#### for each문
```java
for(타입 변수명 : 배열명){
	...
}
```
```java
int[] array = {1, 2, 3, 4, 5};
		
for(int i = 0; i < array.length; i++) { // i : 0, 1, 2, 3, 4
System.out.println(array[i]);
}
		
// 위 for문과 같은 기능
for (int i : array) {
System.out.println(i);
}
```

#### Arrays
- 배열을 다룰 때 사용하는 유틸리티

- copyOf : 배열 복사 
```java
int[] copyFrom = {1, 2, 3};
		
int[] copyTo = java.util.Arrays.copyOf(copyFrom, copyFrom.length);
//copyFrom과 copyTo는 서로 다른 배열 인스턴스 참조
		
for(int c : copyTo) {
System.out.println(c);
}
System.out.println("-------------------");
int[] copyTo2 = java.util.Arrays.copyOf(copyFrom, 5);
		
for(int c : copyTo2) {
System.out.println(c);
}
System.out.println("-------------------");
int[] copyTo3 = copyFrom; 
// copyTo3와 copyFom은 같은 배열 인스턴스 참조 
for(int c : copyTo3) {
System.out.println(c);
}
```
> 배열 복사와 같은 배열을 참조하는 것은 다르다!

## section 5.2 배열 2편
- copyOfRange : 부분 복사 
```java
char[] copyFrom = {'h', 'e', 'l', 'l', 'o', '!'};
		
char[] copyTo = Arrays.copyOfRange(copyFrom, 1, 4);
		
for(char c : copyTo) {
System.out.println(c); // e l 출력 
}
```
- compare : 두 개의 배열 비교
```java
public static void main(String[] args) {
		int[] array1 = {2, 2, 3, 4, 5};
		int[] array2 = {1, 2, 3, 4, 5};
		
		// 양수, 0, 음수
		// 왼쪽 배열이 더 크면 양수
		// 같으면 0
		// 오른쪽 배열이 더 크면 음수
		
		int compare = Arrays.compare(array1, array2);
		System.out.println(compare); // 1 출력 

	}
```

- sort : 정렬
```java
int[] array = {5, 4, 3, 1, 2};
Arrays.sort(array);
for (int i : array) {
	System.out.println(i) // 1 2 3 4 5로 정렬
}
```

- binarySearch : 숫자 검색 

```java
int[] array = {5, 4, 3, 1, 2};
		
Arrays.sort(array); // binarySearch 전에 정렬해주기 
int i = Arrays.binarySearch(array, 4); // 배열에서 4 찾기
System.out.println(i); // 4가 들어 있는 인덱스 출력 
```



#### 명령 행 아규먼트 (Command-Line Arguments)
- 강좌에서 많이 사용된 배열이 무엇인지 물어본다면 그것은 main메소드에 있는 String[] args
- main 메소드는 JVM이 실행하는 메소드이다
- JVM이 메인 메소드를 실행할 때 String[]을 아규먼트로 넘겨 준다는 것을 의미한다


```java
public class commandLIneExam {
	public static void main(String[] args) {
		System.out.println(args.length); // 0 출력
		// String[] args = new String[0];
		// main(args);
	}
}
```


#### 제한 없는 아규먼트(unlimited arguments)
- 경우에 따라서 메소드 아규먼트를 가변적으로 전달하고 싶은 경우가 있다
- 메소드에 정수값을 경우에 따라 3개, 어떤 경우엔 5개를 넘기고 싶다면 어떻게 해야할까?

```
리턴타입 메소드이름(타입... 변수명){
	...
}
```
```java
public static void main(String[] args) {
System.out.println(sum(5, 10));
System.out.println(sum(1, 2, 4, 2));
System.out.println(sum(3, 1, 2, 3, 4, 5));
}
	
public static int sum(int... args) { // args 배열로 처리 
System.out.println("print1 메소드 - args길이 : " + args.length);
int sum = 0;
for(int i = 0; i <args.length; i++) {
sum += args[i];
}
return sum;
}
```

## section 5.3 제네릭과 컬렉션 프레임워크
#### 제네릭(Generic)
- T는 제네릭과 관련된 부분
- 제네릭은 클래스 이름 뒤나, 메소드의 리턴타입 앞에 붙을 수 있다
- <T> 부분은 T라는 이름의 제네릭 타입을 선언한다는 것을 의미
- T는 Type의 약자기 때문에 많이 사용되는 문자로 다른 단어 사용 가능

 ```java
public class GenericBox<T> {
	private T t;
	public void add(T obj) {
		this.t = obj;
	}
	
	public T get() {
		return this.t;
	}
	
	public static void main(String[] args) {
		GenericBox<String> genericBox = new GenericBox<>();
		genericBox.add("kim");
		String str = genericBox.get();
		System.out.println(str.toUpperCase());
	}
}
```

#### 제네릭의 장점
- 정해진 타입만 사용하도록 강제할 수 있다
- 타입을 강제함으로써 컴파일할 때 잘못된 타입의 값이 저장되는 것을 막을 수 있다


#### 컬렉션 프레임워크(Collection Framework)
- Java Collectin Framework라고 불리워지는 Colletions API는 Java 2부터 추가된 자료구조 클래스 패키지를 말한다
- 자료를 다룰 때 반드시 필요한 클래스의 모음으로써 JAVA 프로그래머라면 꼭 숙지하고 있어야만 한다



**컬렉션 프레임워크의 핵심 인터페이스**(암기!)       
<img src="https://github.com/suuxxirr/STUDY/assets/102400242/cd71725a-11b8-49d9-969f-69385a5353cc">             
(점선 : 구현, 실선 : 상속)

**java.util.Collection 인터페이스**
- java.util.Collection 인터페이스는 컬렉션 프레임워크 가장 기본이 되는 인터페이스
- 해당 인터페이스는 순서를 기억하지 않고, 중복을 허용하여 자료를 다루는 목적으로 만들어졌다


**java.util.List 인터페이스**
- java.util.List 인터페이스는 순서가 중요한 자료를 다룰 때 사용하는 인터페이스
- Collection을 상속받음으로써 Collection이 가지고 있는 add(), size(), iterator() 메소드를 사용할 수 있다
- 해당 인터페이스는 순서를 알고 있다고 가정하기 때문에 특정 순서로 저장된 자료를 꺼낼 수 있는 get(int) 메소드를 가지고 있다


**java.util.Set 인터페이스**
- java.util.Set 인터페이스는 중복을 허용하지 않는 자료를 다룰 때 사용하는 인터페이스
- 같은 값을 허용하지 않는다는 것은 같은 값을 저장할 수 없다는 것이다
- 같은 값을 여러번 추가하여도 마지막 값 하나만 저장된다
- Set 인터페이스에 저장되는 객체들은 Object가 가지고 있는 equals() 메소드와 hashCode() 메소드를 오버라이딩해야한다

**java.util.Iterator 인터페이스**
- java.util.Iterator 인터페이스는 자료구조에서 자료를 꺼내기 위한 목적으로 사용되는 인터페이스
- Iterator 패턴을 구현하고 있다

**java.util.Map 인터페이스**
- java.util.Map 인터페이스는 키(Key)와 값(Value)를 함께 저장하기 위한 목적으로 만들어진 인터페이스
- 같은 Key값으론 하나의 값만 저장할 수 있다



ArrayList 예제
```java
import java.util.ArrayList;

public class ListExam {
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<>();
		list.add("kim");
		list.add("lee");
		list.add("hong");
		
		String str1 = list.get(0);
		String str2 = list.get(1);
		String str3 = list.get(2);
		
		System.out.println(str1);
		System.out.println(str2);
		System.out.println(str3);
	}
}
```

Collection & Iterator 예제
```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class ListExam02 {
	public static void main(String[] args) {
		Collection<String> collection = new ArrayList<>();
		collection.add("Kim");
		collection.add("lee");
		collection.add("hong");
		
		System.out.println(collection.size());
		
		Iterator<String> iter = collection.iterator();
		while(iter.hasNext()) {
			String str = iter.next();
			System.out.println(str);
		}
	}
}
```


- Set 예제

```java
import java.util.*;

public class SetExam {
	public static void main(String[] args) {
		Set<String> set = new HashSet<>();
		set.add("hello");
		set.add("hi");
		set.add("hong");
		
		Iterator<String> iter = set.iterator();
		while(iter.hasNext()) {
			String str = iter.next();
			System.out.println(str);
		}
	}
}
```
```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Objects;
import java.util.Set;


public class SetExam2 {
	public static void main(String[] args) {
		Set<MyData> mySet = new HashSet<>();
		mySet.add(new MyData("kim", 500));
		mySet.add(new MyData("lee", 200));
		mySet.add(new MyData("hong", 700));
		
		Iterator<MyData> iterator = mySet.iterator();
		while(iterator.hasNext()) {
			MyData myData = iterator.next();
			System.out.println(myData);
		}
		
		
	}
}



class MyData{
	private String name;
	private int value;
	
	public MyData(String name, int value) {
		this.name = name;
		this.value = value;
	}
	public String getName() {
		return name;
	}
	public int getValue() {
		return value;
	}
	@Override
	public int hashCode() {
		return Objects.hash(name, value);
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		MyData other = (MyData) obj;
		return Objects.equals(name, other.name) && value == other.value;
	}
	
	
}
```

> hash 알고리즘이 나오면 Object가 가지고 있는 hashCode(), equals() 메소드를 오버라이딩해줘야한다

Map 예제
```java
import java.util.HashMap;
import java.util.Map;

public class MapExam {
	public static void main(String[] args) {
		Map<String, String> map = new HashMap<>();
		map.put("k1", "Hello");
		map.put("ke", "hi");
		map.put("k3", "안녕");
		
		System.out.println(map.get("k1"));
		System.out.println(map.get("k2"));
		System.out.println(map.get("k3"));

		Set<String> keySet = map.keySet();
		Iterator<String> iterator = keySet.iterator();
		while(iterator.hasNext()) {
			String key = iterator.next();
			String value = map.get(key);
			
			System.out.println(key + " : " + value);

	}
}
```



















