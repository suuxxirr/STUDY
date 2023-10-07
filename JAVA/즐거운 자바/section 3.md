# section 3. 객체지향 문법 2/3
## section 3.1 인스턴스 필드
#### 클래스 메소드 vs 인스턴스 메소드
- static이 붙은 메소드 = 클래스 메소드
- static이 붙지 않은 메소드 = 인스턴스 메소드
- 인스턴스 별로 다르게 동작해야 한다면 인스턴스 메소드
- static 메소드는 객체 생성이나 유틸리티 관련에서 사용될 때가 있다
- 되도록 인스턴스 메소드 사용

#### 필드(field)
- 클래스가 가지는 속성을 자바 언어에서는 필드라고 한다
- 필드는 어떤 키워드와 함께 사용하느냐에 따라서 사용방법이 달라진다
- static이라는 키워드가 함께 사용되는 필드 = 클래스 필드
- static이라는 키워드가 함께 사용되지 않는 필드 = 인스턴스 필드


#### 필드 선언 방법
```
[접근제한자] [static] [final] 타입 필드명 [=초기값];
```

- 접근제한자는 public, protected, 아무것도 없는 경우 (default), private이 올 수 있다
- 필드는 첫번째 글자를 소문자롤 시작하는 것이 관례
- 타입은 기본형(boolean, byte, char, short, int, long, float, double)과 참조타입(class, 인터페이스, 배열) 등이 나올 수 있다
- 초기값이 없을 경우 참조형일 경우 null로, boolean형일 경우 false로, 나머지 기본형은 모두 0으로 초기화된다

#### 필드 선언 예제
```java
String name;
String address = "경기도 고양시";
public int age = 50;
protected boolean flag;
```


## section 3.2 클래스 필드

```java
public class Person {
	String name;
	String address;
	boolean isVip;
	static int count = 0;
}
```

```java
public class PersonTest {
	public static void main(String[] args) {
		Person p1 = new Person();
		Person p2 = new Person();
		p1.name = "홍길동";
		p2.name = "조조";
		
		System.out.println(p1.name); // 홍길동
		System.out.println(p2.name); // 조조
		System.out.println(p1.count); // 0
		System.out.println(p2.count); // 0
		p1.count++;
		System.out.println(p1.count); // 1
		System.out.println(p2.count); // 1
		p2.count++;
		System.out.println(p1.count); // 2
		System.out.println(p2.count); // 2

    System.out.println(Person.count); / 2
	}
}
```
Person p1  = new Person(); 라인에서    
Person 클래스 정보를 읽어 들일 때, **static한 필드는 정적 영역에 따로 저장된다**    
Person이 가지고 있는 count변수는 별도로 저장되고 0으로 초기화된다    
=> **count변수는 인스턴스별로 가지는 것이 아니라 정적 영역에 따로 관리된다!**    
=> 위 코드에서 p1.count와 p2.count는 같은 메모리 영역의 count기 때문에 p1.count를 증가시켜도 p2.count도 증가    

인스턴스를 만들지 않아도 Person을 JVM이 읽어들일 때 count변수는 메모리에 따로 올라간다
=> Person.count로 사용하는 것이 좋다

## section 3.3 클래스 메소드에서 인스턴스 필드를 사용하지 못하는 이유 & static block

 > 클래스 메소드 안에서 인스턴스 필드를 사용하려고 하면 컴파일 오류가 발생한다

```java
public class Person {
	String name; // 인스턴스 필드
	String address; // 인스턴스 필드
	boolean isVip; // 인스턴스 필드
	static int count = 0; // 클래스 필드
	
	public void printName() { // 인스턴스 메소드
		System.out.println("내 이름은 "+ name);
	}
	
	public static void printCount() { // 클래스 메소드
		System.out.println("count : " + count);
		System.out.println("name"); // static한 메소드 안에서는 인스턴스 필드나, 인스턴스 메소드를 사용할 수 없다
		// 메모리에 생성되는 시점이 다르기 때문에...
	}
}
```


> 클래스 필드는 static 블록에서 초기화할 수 있다

```java
public class Person {
	String name; // 인스턴스 필드
	String address; /
	boolean isVip; 
	static int count; // 클래스 필드
	static { // 클래스 필드 static 블록에서 초기화
    count = 100;
  }
	public void printName() { // 인스턴스 메소드
		System.out.println("내 이름은 "+ name);
	}
	
	public static void printCount() { // 클래스 메소드
		System.out.println("count : " + count);
		System.out.println("name"); // static한 메소드 안에서는 인스턴스 필드나, 인스턴스 메소드를 사용할 수 없다
		// 메모리에 생성되는 시점이 다르기 때문에...
	}
}

```

## section 3.4 main 메소드보다 먼저 실행되는 static 블록
> static 블록은 main 메소드보다 먼저 실행된다 
```java
public class Hello2 {
	static int i;
	static {
		i = 500;
		System.out.println("static block");
	}
	public static void main(String[] args) {
		System.out.println("Hello");
	}
	// static block이 먼저 출력되고 Hello가 출력된다
}
```

## section 3.5 java의 메모리 영역
<img width="488" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/570ccab0-3641-4850-93e6-f21715355c22">

소스코드, 클래스 파일 자체는 정적이다
- 동적인 것들은 실행되면서 생성되는 것들을 말한다
- 클래스 정보 자체는 정적이다 


#### 자바 초보자를 위한 암기사항
- new 연산자를 사용할 때마다 메모리에 인스턴스가 생성된다
- 인스턴스는 더 이상 참조되는 것이 없을 때, 나중에 (언제될지는 모름. 보토 메모리가 부족할 때) 가비지 컬렉션이 된다
- static한 필드는 클래스가 로딩될 때 딱 한 번 메모리에 올라가고 초기화된다
- 인스턴스 메소드는 인스턴스를 생성하고 나서 레퍼런스 변수를 이용해 사용할 수 있다
- 클래스 메소드는 클래스명.메소드명()으로 사용 가능하다
- 메소드 안에 선언된 변수들은 메소드가 실행될 때 메모리에 생성되었다가, 메소드가 종료될 때 사라진다

## section 3.6 추상화란?
추상화 : 중요한 것은 남기고, 불필요한 것은 제거한다

프로그램을 만들 때는 비즈니스 영역(도메인 영역)에 맞게 추상화를 해야한다

캡슐화 : 관련된 것을 잘 모아서 가지고 있는 것을 캡슐화라고 한다. 관련된 것을 잘 모아서 가지고 있을수록 응집도가 높다고 표현한다


## section 3.7 좋은 객체 vs 나쁜 객체

좋은 객체는 응집도는 높고 결합도(Coupling)는 낮다

좋은 객체란 역할과 책임에 충실하면서 다른 객체와 잘 협력하여 동작하는 객체를 말한다    
나쁜 객체란 여러가지 역할을 한 객체에게 부여하거나, 이름과는 맞지 않는 속성과 기능을 가지도록 하거나 제대로 동작하지 않는 객체를 말한다
또한 다른 객체와도 동작이 매끄럽지 않는 객체를 의미한다

## section 3.8 다형성과 오버로딩
**다형성 - 메소드 오버로딩**
- 메소드의 이름은 같고 매개변수의 갯수나 타입이 다른 함수를 정의하는 것을 의미
- 리턴값만을 다르게 갖는 오버로딩은 작성할 수 없다


## section 3.9 Package
**패키지** : 클래스는 패키지를 이용하여 관련된 클래스들을 관리한다. 자바에서 패키지는 폴더와 거의 같은 기능을 제공한다

**패키지 이름 규칙** : 패키지 이름은 보통 도메인 이름을 거꾸로 적은 후에 프로젝트 이름 등을 붙여서 만든다


#### 패키지 선언 방법
```package 패키지명;```
- 주석문이나 빈 줄을 제외하고 가장 윗 줄에 위와 같은 형식으로 선언

#### 패키지가 정의된 클래스 컴파일 하기
- ```javac -d 경로명 *.java```
- -d 옵션을 사용한다

#### 패키지를 사용할 땐 import 문장 사용
```import 패키지명.클래스명;```

## section 3.10 상속
여러 종류의 객체를 하나의 이름으로 부를 수 있는 것을 일반화라고 한다
- 상속 = 일반화 + 확장
부모 클래스를 상속 받는다는 것은 부모가 가지고 있는 것을 자식이 물려받아 사용할 수 있다는 것을 의미한다    


- 아무것도 상속받지 않으면 자동으로 java.lang.Object를 상속 받는다 (모든 클래스는 Object의 자손이다)

#### 상속 선언 방법
```java
[접근제한자] [abstract|final] class 클래스명 extends 부모클래스명 {
	...
}
```


## section 3.11 상속 - 부모 타입으로 자식 인스턴스 참조하기

> 부모 타입으로 자손 타입을 참조할 수 있다

```java
Car car = new Bus();
// 버스는 자동차
// 버스 인스턴스 생성
// 버스 인스턴스를 생성하는데, 실제 참조는 Car 타입으로 하라는 의미
```
- 참조변수 타입으로 Car를 사용하면, **Car가 가지고 있는 메소드만** 사용 가능하다


**왜 참조 타입을 부모 타입으로 사용할까?**    

<img width="423" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/e017e067-7bd7-4d81-b1e9-80149d6399a5">

사용하는 것은 Bus인데, Car의 기본 기능만 이용하고 싶을 때 ```Car c1 = new Bus();```와 같이 선언     
=> Bus 인스턴스를 생성하고 왜 Car 기능만 이용하려고 할까?      
=> 예시) 오픈카를 고급 식당에 가서 발레파킹을 맡김. 주차 요원이 뚜껑을 열었다 닫았다 하면서 주차를 하면 안됨    
   객체를 사용할 때, 참조 변수의 타입만 보면 어떤 메소드를 사용할지 알게되니 코드를 분석할 때 쉬워진다

## section 3.12 상속 - 다형성(메소드 오버라이딩), 객체의 형변환
#### 다형성 - 메소드 오버라이딩
- 상위 클래스의 메서드를 하위 클래스가 재정의 하는 것
- 메서드의 이름은 물론 파라메터의 갯수나 타입도 동일해야 하며, 주로 상위 클래스의 동작을 상속받은 하위 클래스에서 변경하기 위해 사용된다

> **메소드가 오버라이딩 되면 무조건 자식의 메소드가 실행된다!**


```java
Car car = new Bus();
car.run();
```
- Car도 public void run() 메소드를 가지고 있고, Bus도 public void run() 메소드를 가지고 있다면?


=> Bus의 run() 메소드가 실행된다

```java
public class Car {
	public void run() {
		System.out.println("달리다");
	}
}
```
```java
public class Bus extends Car {
	public void run() {
		System.out.println("버스가 달리다");
	}
	public void 안내방송() {
		System.out.println("안내방송");
	}
}
```
```java
public class CarExam01 {
	public static void main(String[] args) {
		Bus b1 = new Bus();
		b1.run(); // 버스가 달리다 출력
		Car c1 = new Bus(); // 버스는 자동차다
		c1.run(); // 버스가 달리다 출력 , 그(c1) 자동차는 달린다 
	}
}
```
- b1, c1 모두 Bus 인스턴스가 만들어졌다
- Bus 인스턴스를 Bus 타입으로 참조하나 Car 타입으로 참조하나 run() 메소드를 호출하면 "버스가 달리다" 출력

=> **메소드가 오버라이딩 되면 무조건 오버라이딩 된 메소드가 실행된다⭐**

#### 객체의 형변환
```Car c1 = new Bus();```로 선언하면, Bus가 여러가지 메소드가 있어도 Car가 가지고 있는 메소드만 사용 가능    
=> Bus가 가지고 있는 원래 메소드가 사용하고 싶다면?
=> 객체 형변환 사용

```java
public class CarExam01 {
	public static void main(String[] args) {
		Bus b1 = new Bus();
		b1.run(); // 버스가 달리다 출력
		Car c1 = new Bus(); // c1은 Car 타입 
		c1.run();

		Bus b2 = (Bus) c1; // c1이 참조하는 Bus 인스턴스를 Bus 타입으로 변환해서 b2가 참조하라는 의미 
		b2.안내방송(); // 에러 안 남
	}
}
```

<img width="402" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/7588d168-77b1-466f-ae51-50f8c7c8c984">

## section 3.13 상속 - 필드와 메소드 오버라이딩 

> **⭐필드는 Type을 따라가고, 메소드는 오버라이딩 된 자식의 메소드가 실행된다**     

```java
public class Parent {
	public int i = 5;
	public void printI() {
		System.out.println("parent - printI() : " + i);
	}
}
```
```java
public class Child extends Parent {
	public int i = 15; // 필드에 대한 오버라이딩 
	public void printI() { // 메소드에 대한 오버라이딩 
		System.out.println("child - printI() : " + i);
	}
}
```

```java
public class Exam01 {
	public static void main(String[] args) {
		Parent p1 = new Parent();
		System.out.println(p1.i); // 5
		p1.printI(); // child - printI() : 5
		System.out.println("------------------");
		Child c1 = new Child();
		System.out.println(c1.i); // 15
		c1.printI(); // child - printI() : 15 
		System.out.println("------------------");
		Parent p2 = new Child();
		System.out.println(p2.i); // 5
		p2.printI();  // child - printI() : 15 
	}
}
```

## section 3.14 상속 - Getter & Setter 프로퍼티(property)

private한 필드에 접근하기 위해(필드의 값을 수정하고 얻기 위해) 제공하는 메소드를 setter, getter라고 한다

- 이클립스 getter, setter 단축키 : ```Alt + Shift + S => R```

```java
public class Book {
	private int price; // field price 

	// setter, getter - 프로퍼티(property) - price 프로퍼티 
	public int getPrice() {
		return this.price; // this는 내 자신 인스턴스를 창조하는 예약어 
	}

	public void setPrice(int price) { // 지역변수 price 
		this.price = price;
	}
}
```
- price : 지역변수
- this.price : 인스턴스 변수
- setPrice() : 매개변수로 받은 지역변수 price로 this.price를 초기화


```java
public class BookExam01 {
	public static void main(String[] args) {
		Book b1 = new Book();
		
		b1.setPrice(500);
		System.out.println(b1.getPrice()); // 500 출력 
	}
}
```


## section 3.15 상속 - 오버라이딩 하라고 준비되어 있는 Object의 toString(), equals(), hashCode() 메소드
#### Object가 오버라이딩하라고 제공하는 메소드
- toString()
- equals() & hashCode()

#### toString()

Car 클래스
```java
public class Car {
	public void run() {
		System.out.println("달리다");
	}
}
```
```java
public class CarExam3 {
	public static void main(String[] args) {
		Car c1 = new Car();
		System.out.println(c1); // Car@2ff4acd0 출력 
	}
}
```

println() 메소드는 자바 개발자가 만든 메소드. Car 타입의 변수를 받아들여도 에러가 나지 않고 있다    
=> println 메소드는 오버로딩 된 메소드 중 어떤 매개변수 사용되는 걸까?   

<img width="356" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/0406aeab-e1aa-4ff0-a352-b529e41f4473">
(java API 참고)


=> car를 받아들이 수 있는건 println(Object x) 메소드
= Object로 참조할 수 있는 것은 무엇이든 받을 수 있다
= Object의 자손들은 다 올 수 있다
- println(Object x)  : Object가 갖고있는 toString 메소드를 출력해준다
- ```System.out.println(o1.toString());``` == ```System.out.println(o1));``` 

아무것도 상속받지 않으면 자동으로 Object를 상속 받는다 
=> 때문에 이러한 코드가 가능하다 
```java
Object o1 = new Bus();
Object o2 = new Car(); // 에러 발생 x
```


```java
public class Car {
	public void run() {
		System.out.println("달리다");
	}
	
	@Override
	public String toString() {
		return "자동차!!";
	}
}
```
```java
public class CarExam3 {
	public static void main(String[] args) {
		Car c1 = new Car();
		System.out.println(c1); // '자동차!!' 출력
	}
}
```
> Object가 갖고있는 toString 메소드를 오버라이딩함으로써 내가 만든 객체를 출력했을 때 내가 원하는 문자열을 출력할 수 있다 

#### equals() & hashCode()

예시) Book 클래스가 있을 때, book 인스턴스 2개를 만들었을 때, 두 인스턴스가 같다고 말할 수 있으려면 
**기준**을 정해야한다 (예 : 제목, 저자, 출판날짜)
- equals 메소드는 같은 값이냐를 비교해준다
- 비교하려면 기준을 정해야한다 => **equals 메소드는 그냥 쓰면 안되고, 오버라이딩해서 써야한다**

hash 기능을 사용할 때 equals 메소드를 오버라이딩해야한다 

