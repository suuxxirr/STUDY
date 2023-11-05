# section 4. 객체지향 문법 3/3
## section 4.1 생성자

#### 생성자 
- 인스턴스를 생성할 때 사용
- 어떤 값을 가지고 인스턴스가 만들어지게 하고 싶다면 생성자를 사용
- 클래스 작성시 생성자를 하나도 만들지 않았다면 자동으로 (아무 일도 안 하는) 기본 생성자가 생성된다
- 기본 생성자는 매개변수를 하나도 받지 않는 생성자를 말한다


- 클래스 이름과 같아야 한다
- return type이 없다

- 이클립스 생성자 단축키 : ```Alt + Shift + S => O```

 
 생성자를 하나라도 만들어 주면 기본 생성자가 만들어지지 않는다  

  - 생성자에서 넣어준 값을 리턴해주는 기능만 가진 객체를 **불변객체**라고 한다


#### 생성자 오버로딩
- 생성자는 매개변수의 개수가 다르거나, 타입이 다르다면 여러개를 가질 수 있다


User 클래스
```java
public class User {
	private String email;
	private String password;
	private String name;
	
	// 생성자1
	public User(String name, String email) {
		this.email = email;
		this.name = name;
	}
	// 생성자 오버로딩 
	public User(String name, String email, String password) {
		this.email = email;
		this.password = password;
		this.name = name;
	}

// toString 오버라이딩
	@Override
	public String toString() {
		return "User{" + "email='" + email + '\'' + ", name ='" + name + '\'' + '}';
	}
}
```
```java
public class UserExam {
	public static void main(String[] args) {
		User user = new User("김수현", "abcd@gamil.com");
		
		System.out.println(user); //toString 메소드 자동호출
	}
}
```



#### this() 생성자 
> 생성자 안에서 자기 자신의 생성자를 호출할 수 있다 => 코드 중복 해결 

> 자신의 생성자를 호출할 때는 this()를 사용한다

- this는 인스턴스 자기 자신을 참조할 때 사용하는 키워드
- this() 생성자는 자기 자신의 생성자를 말한다
- this() 생성자는 생성자 안에서만 사용 가능하다
- this() 생성자는 생성자 안에서 super() 생성자를 호출하는 코드 다음이나, 첫번째 줄에 위치해야한다 
- 많이 받아들이는 쪽의 생성자를 this로 호출해주는 것이 좋다


 


```java
public class User {
	private String email;
	private String password;
	private String name;
	
	// 생성자
	public User(String name, String email) {
		this(name, email, null); // 생성자 호출 
	}
	// 생성자 오버로딩 
	public User(String name, String email, String password) {
		super();
		this.email = email;
		this.password = password;
		this.name = name;
	}
}
```

#### super() 생성자 
> 부모의 생성자를 호출할 때는 super()를 사용한다 

- super는 인스턴스 부모를 참조할 때 사용하는 키워드
- super() 생성자는 부모 생성자를 의미한다
- super() 생성자는 생성자 안에서만 사용 가능하다
- super() 생성자는 생성자 안에서 첫번째 줄에만 올 수 있다
- 생성자는 무조건 super() 생성자를 호출해야한다 
 사용자가 super() 생성자를 호출하는 코드를 작성하지 않았다면 자동으로 부모의 기본 생성자가 호출된다
- 부모 클래스가 기본 생성자를 가지고 있지 않다면, 사용자는 반드시 직접 super() 생성자를 호출하는 코드를 작성해야한다

```java
public class Car2 {
	public Car2() {
		System.out.println("Car2() 생성자 호출");
	}
}
```
```java
public class Bus2 extends Car2 {
	
}
```
```java
public class Car2Exam {

	public static void main(String[] args) {
		Car2 c1 = new Car2(); //"Car2() 생성자 호출" 출력
		Bus2 b1 = new Bus2(); //"Car2() 생성자 호출" 출력 
	}

}
```
 Bus2 클래스에 생성자가 없으면,   
=> ```public Bus2() {}```생성자가 자동으로 만들어진다(생성자가 하나라도 만들어지면 기본 생성자가 만들어지지 않는다)      
=> 생성자에 (컴파일 때)자동으로 들어가는 코드 : ```super();```       



```java
public class Bus2 extends Car2 {
	public Bus2() {
		super(); // 부모의 기본 생성자를 호출하는 코드가 자동으로 삽입된다 
		System.out.println("Bus2기본 생성자 ");
	}
}
```


부모가 기본 생성자가 없을 때는 자식 클래스에서 super() 호출하고 값(매개변수)을 넣어줘야 한다 
(강의 30분 참고) 

## section 4.2 추상 클래스 
#### 추상 클래스
- 추상 클래스는 인스턴스가 될 수 없다
- 추상 클래스를 상속받는 자손이 인스턴스가 된다 
- abstract 키워드를 사용하여 클래스를 정의한다
- 추상 클래스는 보통 1개 이상의 추상 메소드를 가진다 (추상 메소드가 없어도 오류가 발생하진 않음)
- ```public abstract class 클래스명 {...}```

```java
public abstract class Car2 {
	public Car2(String name) {
		System.out.println("Car2() 생성자 호출");
	}
	
	// 추상 메소드
	public abstract void run(); // run()은 자동차마다 다르게 구현할 것 같아서 추상 메소드로
}
```
> 부모가 가지고 있는 추상 메소드는 자식에서 반드시 구현 해주어야 한다


추상 메소드가 있는 Car2를 상속받은 Bus2 클래스에서는 반드시 추상 메서드를 구현해줘야 한다
```java
public class Bus2 extends Car2 {
	public Bus2() {
		super("Bus!"); 
		System.out.println("Bus2기본 생성자 ");
	}

	@Override
	public void run() {
		System.out.println("후륜구동으로 작동한다");
		
	}	
}
```

## section 4.3 템플릿 메소드 패턴으로 추상 클래스 개념 익히기 

추상 클래스는 템플릿 메소드 패턴(Template Method Pattern)에서 많이 쓰인다

      


```java
package com.example.fw;



public abstract class Controller {
	public void init() {
		System.out.println("초기화 하는 코드");
	}
	
	public void close() {
		System.out.println("마무리 하는 코드");
	}
	
	public abstract void run(); // 매번 다른 코드
	
	// 내가 가지고 있는 메소드를 호출
	// 어떤 순서를 가지고 있다 => 이런 메소드를 >>>>템플릿 메소드<<<<라고 한다 
	public void execute() {
		this.init();
		this.run();
		this.close();
	}	
}
```
```java
package com.example.myproject;
import com.example.fw.Controller;

public class FirstController extends Controller {

	@Override
	public void run() {
		// TODO Auto-generated method stub
		System.out.println("별도로 동작하는 코드");
		
	}
	
}
```
- Controller를 상속받게 함으로써 매번 변하는 코드만 구현하도록 하였다 
```java
package com.example.main;

import com.example.fw.Controller;
import com.example.myproject.FirstController;

public class ControllerMain {
	public static void main(String[] args) {
		Controller c1 = new FirstController();
		c1.execute();
		/* 초기화 하는 코드
		   별도로 동작하는 코드
		   마무리 하는 코드 */ // 출력 
	}
}
```

## section 4.4 final 클래스, 불변객체 String

부모가 될 수 없는 클래스 
- 상속을 금지 시키려면 클래스를 정의할 때 **fianl 키워드**를 사용한다
- ```public final class 클래스명 {...}```

```java
public class StringExam {
	public static void main(String[] args) {
		String str1 = "hello";
		String str2 = "hello";
		String str3 = new String("hello");
		String str4 = new String("hello");
		
		if (str1 == str2)
			System.out.println("str1 == str2"); // 출력 o
		if (str1 == str3)
			System.out.println("str1 == str3"); // 출력 x
		if (str3 == str4)
			System.out.println("str3 == str4"); // 출력 x
		
		System.out.println(str1); // hello
		System.out.println(str2); // hello 
		System.out.println(str3); // hello 
		System.out.println(str4); // hello 
	}
}
```
- 레퍼런스 타입에서 ```==```은 같은 것을 참조하느냐는 뜻 
- hello 문자열은 같은 걸 참조
- new가 쓰이면 항상 heap 메모리에 새로운 객체가 만들어진다
- 값을 비교하려면 eqauls 메소드 사용 (str1.equals(str2))

> String이 가지고 있는 모든 메소드는 자기 자신을 변하게 하지 않는다





## section 4.5 접근제한자 
<img src="https://github.com/suuxxirr/STUDY/assets/102400242/b053a1b0-a08b-4c39-a29d-66d2a9cc8abe">


## section 4.6 인터페이스 & 로또번호 생성기 만들기 
만들어야 할 기능들을 관련된 것끼리 묶은 후 이름을 지어주는 것을 인터페이스라고 한다 

#### 인터페이스 작성 문법 
```java
[public] interface 인터페이스이름 { ... }

// 예시
public interface User { ... }
```
- "인터페이스 이름"은 Upper CamelCase로 작성된다
- interface도 확장자가 .java 파일로 작성한다
- 인터페이스의 모든 필드는 **public static final**이어야 하며, 모든 메소드는 **public abstract**이어야 한다
- (java7까지는) final, abstract를 생략하면 자동으로 붙는다
- Java 8부터는 디폴트 메소드와 정적 메소드 선언도 가능하다



## section 4.8 팩토리 메소드 패턴, Java Reflection

> 팩토리 메소드 패턴은 객체가 생성되는 과정을 숨겨주는 패턴 

#### 클래스 로더를 이용한 인스턴스 생성하기
```java
Class clazz = Clas.forName("클래스풀네임)";
Object obj = clazz.newInstance();
```
```java
package com.example;

public class Bus {
	public void a() {
		System.out.println("a");
	}
	public void b() {
		System.out.println("b");
	}
	public void c() {
		System.out.println("c");
	}
}
```
```java
package com.example;

import java.lang.reflect.Method;

public class ClassLoaderMain {

	public static void main(String[] args) throws Exception {
		// a() 메소드를 가지고 있는 클래스가 있다 
		// 이 클래스 이름이 아직 무엇인지 모른다 
		// 나중에 클래스 이름을 알려주겠다
		// a() 메소드를 실행할 수 있는 코드를 작성하여라
		// => 이럴 때 클래스 로더 사용 
		
		
		// className에 해당하는 클래스 정보를 CLASSPATH에서 읽어들이고,
		// 그 정보를 clazz가 참조하도록 한다 
		String className = "com.example.Bus";
		Class clazz = Class.forName(className);
		Object o = clazz.newInstance();
		// 위 세 줄은 Object o = new Bus();와 같은 코드 
		Bus b = (Bus)o; // 형변환 가능 
		b.a();
		
		
		Method[] declareMethods = clazz.getDeclaredMethods(); 
		// clazz.getDeclaredMethods() => clazz가 가지고 있는 메소드 정보가 리턴된다 
		for(Method m : declareMethods) {
			System.out.println(m.getName());
		}
	}

}
```


## section 4.9 익명 클래스(Anonymous Clsass), 람다(Lamda)


#### 익명 클래스 
- new 생성자() { ... }
- 생성자 뒤에 중괄호가 나오고 코드를 오버라이딩하여 보통 구현한다

```java
Car car = new Car() {
	public void run() {
		System.out.println("Car를 상속받는 이름 없는 객체가 run 메소드를 오버라이딩함");
	}}
```

예시 
```java
package com.example;

public abstract class Car {
	public abstract void a();
}
```
```java
package com.example;

public class CarExam {
	public static void main(String[] args) {
		Car c1 = new Car() {
			public void a() {
				System.out.println("이름 없는 객체의 a() 메소드 오버라이딩");
			}
		};
	}
}
```



#### 람다 인터페이스
- 메소드를 하나만 가지고 있는 인터페이스
- 람다 인터페이스를 사용하는 람다 표현식은 JDK 8에서 추가되었다
- JDK 8에 추가된 이러한 문법들을 사용할 때 보통 모던 자바라고 한다













