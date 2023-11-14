# section 6. 주석문, 예외처리, enum
## section 6.1 주석문
#### 자바의 주석문
- //
- /* .....*/
- /** ....*/ : //**와 */ 사이의 내용이 모두 주석처리 된다. JavaDoc 주석문이라고 한다

#### JavaDoc 주석문에서 사용하는 태그들

|annotation|설명|
|------|-------|
|@version|클래스나 메소드의 버전|
|@author|작성자|
|@deprecated|더이상 사용되지 않거나, 삭제될 예정|
|@since|언제 생성, 추가, 수정되었는가?|
|@see|외부 링크나 텍스트, 다른 필드나 메소드를 링크할 대 사용|
|@link|see와 동일한 기능. 링크 제공|
|@exception|발생할 수 있는 Exception 정의|

## section 6.2 예외(Exception) 처리하기 
#### Error의 종류
- 컴파일 에러 : 컴파일시 발생하는 에러
- 런타임 에러 : 실행시 발생하는 에러

자바에서는 실행시 2가지 형태의 오류가 발생할 수 있다
- Error : 수습할 수 없는 심각한 오류
- Exception : 예외 처리를 통해 수습할 수 있는 덜 심각한 오류
- 메모리 부족, 스택오버플로우 등이 발생하여 프로그램이 죽는 것은 프로그래머가 제어할 수 없다


#### 예외 처리하기 (try-catch)
```java
try{
  코드1
  코드2
} catch(Exception클래스명1 변수명1){
    Exception을 처리하는 코드
} catch(Exception클래스명2 변수명2){
    Exception을 처리하는 코드
}
```


 

```java
public class Exception {
	public static void main(String[] args) {
		ExceptionObj1 exobj = new ExceptionObj1();
		int value = exobj.divide(10,0);
		System.out.println(value);
	}
}

class ExceptionObj1{
	public int divide(int i, int k) {
		int value = 0;
		try {
			value = i / k;
		} catch(ArithmeticException ex){
			System.out.println("0으로 나눌 수 업습니다");
		}
		return value;
	}
}
```
<img width="200" src="https://github.com/suuxxirr/STUDY/assets/102400242/d7ee8b3c-3b0e-4197-9178-77e881d6b073">



#### 예외 떠넘기기(throws)
throws : 메소드 선언부 끝에 작성. 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘긴다 
- throws 키워드가 붙어있는 메소드는 반드시 try 블록 내에서 호출되어야 한다
- 그리고 catch 블록에서 떠넘겨 받은 예외를 처리해야 한다

<img src="https://github.com/suuxxirr/STUDY/assets/102400242/41e62c6b-de65-48ec-ba37-3181055fb888">


```java
리턴타입 메소드명(아규먼트 리스트) throws Exception클래스명1, Exception클래스명2 ....{
  코드1
  코드2
  ......
}
```

```java
public class Exception {
	public static void main(String[] args) {
		ExceptionObj1 exobj = new ExceptionObj1();
    try {
        int value = exobj.divide(10,0);
		    System.out.println(value); // 사용되지 않음!
    } catch(ArithmeticException ex) {
      System.ou.println("0으로 나눌 수 없습니다");
    }
	}
}

class ExceptionObj1{
	public int divide(int i, int k) throws ArithmeticException {//divide 메소드를 호출한 쪽에 exception 떠넘기
		int value = 0;
    value = i / k;
		return value;
	}
}
```
<img src="https://github.com/suuxxirr/STUDY/assets/102400242/de0b7488-5095-4270-bb77-85aa3f7a11bb">




#### RuntimeException과 Checked Exception
<img width="200" src="https://github.com/suuxxirr/STUDY/assets/102400242/bf58b2d2-7c7a-45b9-a3e4-735620cdd16a">




- RuntimeException        
: RuntimeException을 상속받고 있는 Exception은 전부 RuntimeException   
: 처리를 해주지 않아도 컴파일이 잘된다          
- CheckedException        
: 그 이외(RuntimeException을 상속받지 않는)는 전부 CheckedException           
: 처리해주지 않으면 컴파일 오류가 발생한다



#### 다중 Exception 처리 

```java
public class Exception6 {
	public static void main(String[] args) {
		int[] array = {4, 0};
		int[] value = null;
		try {
			value[0] = array[0] / array[1];
		}catch(ArrayIndexOutOfBoundsException aiob) {
			System.out.println(aiob.toString());
		}catch(ArithmeticException ae) {
			System.out.println(ae.toString());
		}catch(Exception ex) {
			System.out.println(ex);
		}
	}
}
```

#### 사용자 정의 Exception과 예외 발생시키기 (throw)

thorw : 개발자가 직접 예외를 발생시키고 싶을 때 사용 


사용자 정의 Exception 
```java
public class MyException extends RuntimeException {
	// 오류 메시지나, 발생한 Exception을 감싼 결과로 내가 만든 Exception을
	// 사용하고 싶을 때가 많다 
	
	public MyException(String message) {
		 super(message);
	}
	public MyException(Throwable cause) {
		super(cause);
	}
}
```
```java
public class Exception7 {
	public static void main(String[] args) {
		try {
			ExceptionObj7 exobj = new ExceptionObj7();
			int value = exobj.divide(10, 0);
			System.out.println(value);
		}catch (MyException ex) {
			System.out.println("사용자 정의 Exception 발생");
		}
	}
}

class ExceptionObj7{
	public int divide(int i, int k) throws MyException{
		int value = 0;
		try {
			value = i / k;
		}catch(ArithmeticException ae) {
			throw new MyException("0으로 나눌 수 없습니다");
		}
		return value;
	}
}
```
<img src="https://github.com/suuxxirr/STUDY/assets/102400242/e5973c55-c315-4a15-80cd-62ac57ee4f8b">

## section 6.3 enum
enum 
: 자바의 Enum은 Enumeration의 약자로 JDK 5부터 지원하는 기능이다 



#### 상수 사용시의 문제점 

```java
public class DayType {
	public final static int SUNDAY = 0;
	public final static int MONDAY = 1;
	public final static int TUESDAY= 2;
	public final static int WEDNESDAY = 3;
	public final static int THURSDAY = 4;
	public final static int FRIDAY = 5;
	public final static int SATURDAY = 6;
	// DayType 클래스는 final static int로 정의된 상수를 6개 가진다
	
	int today = DayType.SUNDAY;
	// today는 0이라는 숫자값을 가지게 된다 
	 
	//int today = 100;
	// 하지만 위처럼 0~6사이가 아닌 다른 값들, 
	// 즉 정해진 값만 변수에 할당할 수 없다는 문제가 있다
	// No Type-Safety 
	
}
```

#### Enum 사용하기 
- 클래스를 생성하는 것과 같은 방식으로 Enum 생성




- com.example.enumtype 패키지를 생성       
- 생성된 패키지 아래에 Day enum 생성

```java
package com.example.enumtype;

public enum Day {
	SUNDAY,
	MONDAY,
	TUESDAY,
	WEDNESDAY,
	THURSDAY,
	FRIDAY,
	SATURDAY
}
```

Enum 타입을 필드로 가지는 Today 클래스
```java
public class Today {
	private Day day; // day는 Day(Enum type)이 가지고 있는 값만 가질 수 있다 

	public Day getDay() {
		return day;
	}

	public void setDay(Day day) {
		this.day = day;
	}
	
}
```

Today를 사용하는 TodayTest 클래스 
```java
public class TodayTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Today today = new Today();
		today.setDay(Day.SUNDAY);
		System.out.println(today.getDay()); // SUNDAY 출력 
	}

}
```
```today.setDay(Day.SUNDAY);```                      
: today의 setDay() 메소드에는 Enum타입인 Day가 전달되어야 한다             
```today.setDay(100);```                 
: 위와 같은 코드는 사용할 수 없다. 이러한 것을 타입에 안전하다 **Type-Safety**라고 한다 


#### Enum 타입의 특징
- Enum은 타입에 대해 안전하다. 미리 정의된 Enum 변수 안의 상수만을 대입할 수 있다
- switch문에서 사용 가능 

```java
package com.example.enumtype;

public class DaySwitchTest {
	public static void main(String[] args) {
		Day day = Day.SUNDAY;
		
		switch(day) {
		case SUNDAY: // case Day.SUNDAY 라고 쓰면 에러
			System.out.println("일요일");
			break;
		case MONDAY: 
			System.out.println("월요일");
			break;
		default:
			System.out.println("그외");
		}
		
	}
}
```
case 다음에는 Day가 가지고 있는 상수의 이름이 나와야 한다 


- Enum은 생성자를 가질수 있다. 단 생성자는 private해야 한다
- Enum 생성자는 내부에서만 호출가능

```java
package com.example.enumtype;

public enum Gender {
	MALE("XY"),
	FEMALE("XX");
	
	private String chromosome; // 염색체
	private Gender(String chromosome) {
		this.chromosome = chromosome;
	}
}
```
```java
package com.example.enumtype;

public class GenderTest {
	public static void main(String[] args) {
		Gender gender = Gender.MALE;
		
		System.out.println(gender); // MALE 출력 
	}
}
```

#### Enum에 메소드와 변수 선언하기 
- Enum 안에 선언된 메소드나 변수를 가질 수 있다
- Object가 가지고 있는 메소드를 오버라이딩할 수도 있다

```java
package com.example.enumtype;

public enum Gender {
	MALE("XY"),
	FEMALE("XX");
	
	private String chromosome; // 염색체
	private Gender(String chromosome) {
		this.chromosome = chromosome;
	}
	
	@Override
	public String toString() {
		return "Gender{" + "chromosome='" + chromosome + '\'' + "}";
	}
	
	public void print() {
		System.out.println("염색체 정보 : " + chromosome);
	}
}
```
```java
package com.example.enumtype;

public class GenderTest {
	public static void main(String[] args) {
		Gender gender = Gender.MALE;
		
		System.out.println(gender); // Gender{chromosome='XY'}
		gender.print(); // 염색체 정보 : XY 
	}
}
```

#### Enum 값끼리의 비교
- Enum값끼리 비교할 때는 == 를 사용한다
```java
Day day1 = Day.MONDAY;
Day dat2 = Day.MONDAY;
if (day1 == day2){
  System.out.println("같은 요일입니다.");
}
```

#### EnumMap
- EnumMap은 Enum 타입을 키(key)로 사용할 수 있도록 도와주는 클래스
```java
package com.example.enumtype;

import java.util.EnumMap;

public class EnumMapTest {
	public static void main(String[] args) {
		EnumMap emap = new EnumMap(Day.class);
		emap.put(Day.SUNDAY, "일요일이당");
		emap.put(Day.FRIDAY, "불금");
		
		System.out.println(emap.get(Day.SUNDAY));
	}
}
```

#### EnumSet
- EnumSet은 Enum 상수를 Set 자료구조로 다루기 위한 메소드를 제공하는 클래스



#### Enum 인터페이스 구현
- Enum은 인터페이스를 구현하고, 해당 인터페이스를 오버라이딩하여 구현할 수 있다
```java
pacakge com.example.enumtype;

public interface Printer {
  public void print():
}
```
```java
package com.example.enumtype;

public enum Color implements Printer{
  RED("FF0000"),
  GREEN("00FF00"),
  BLUE("0000FF");

  private String rgb;
  private color(String rgb){
    this.rgb = rgb;
}

@Override
public void print() {
  System.out.println("rgb : " + rgb);
  }
}
```

#### Enum 추상메소드
- Enum은 추상메소드를 가질 수 있다
- 추상 메소드를 가질 경우엔 상수를 정의할 때 추상메소드를 함께 구현해줘야 한다


```java
package com.example.enumtype;

public enum Country {
	KOREA{
		public void print() {
			System.out.println("대한민국");
		}
	}, 
	 JAPAN{
		public void print() {
			System.out.println("일본");
		}
	}, 
	US{
		public void print() {
			System.out.println("미국");
		}
	};
	public abstract void print();
}
```
아래쪽에 print()메소드가 추상 메소드로 선언          
이 경우 상수를 적은 후 블록 안에 해당 메소드를 구현해주어야 한다

















