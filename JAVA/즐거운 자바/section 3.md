# section 3. 객체지향 문법 2/3
## section 3.1 인스턴스 필드
#### 클래스 메소드 vs 인스턴스 메소드
- static이 붙은 메소드 = 클레스 메소드
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





