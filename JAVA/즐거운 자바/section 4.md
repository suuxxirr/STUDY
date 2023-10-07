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
<img width="324" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/30c0dd49-9d57-4f88-91e7-56d4a164ca52">


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
=> ```public Bus2() {}```생성자가 자동으로 만들어진다(생성자가 하나다로 만들어지면 기본 생성자가 만들어지지 않는다)      
=> 생성자에 (컴파일 때)자동으로 들어가는 코드 : ```super();```       



```java
public class Bus2 extends Car2 {
	public Bus2() {
		super(); // 부모의 기본 생성자를 호출하는 코드가 자동으로 삽입된다 
		System.out.println("Bus2기본 생성자 ");
	}
}
```
<img width="200" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ce81b97d-c210-4fb0-8db3-6001beaf85b3">

부모가 기본 생성자가 없을 때는 자식 클래스에서 super() 호출하고 값(매개변수)을 넣어줘야 한다 
(강의 30분 참고) 



