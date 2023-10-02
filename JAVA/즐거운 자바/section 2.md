# section 2 객체지향 문법 1/3
## section 2.1 객체지향문법 시작
객체 지향 프로그래밍은 컴퓨터 프로그래밍의 패러다임 중 하나이다
객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것

- 클래스(class)는 설계도면과 같은 역할
- 클래스를 통해 현실에서 만들어지는 것이 오브젝트 or 인스턴스 
- 인스턴스를 특별한 이름으로 불러주고 싶다면? 참조형 변수를 선언. 참조되지 않은 인스턴스는 쓰레기

## section 2.2 클래스 선언하기
#### 클래스 
- 클래스는 필드와 메소드를 가진다
- 필드는 클래스의 속성
- 메소드는 클래스의 기능

#### 클래스 선언 방법
1. 첫 문자가 문자나 '_', '$'의 특수문자로 시작
2. 자바의 예약어는 식별자로 사용할 수 없다
3. 자바의 식별자는 대소문자를 구분
4. 식별자 길이는 제한이 없고, 공백은 포함할 수 없다


#### 프로그래머들간의 관례
1. 클래스 명은 대문자로 시작
2. 단어와 단어가 만날 경우 2번째 단어의 시작은 대문자로 시작 ex) ```HelloWorld```

```java
접근제한자 class 클래스 이름 {
  필드들;
  생성자들;
  메소드들;
}
```
## section 2.3 자판기 클래스 만들기
> static이 붙은 메소드는 클래스 메소드
> > 클래스 메소드는 인스턴스를 생성하지 않아도 사용 가능      
> > = 인스턴스를 만들지 않아도 메모리에 올라가 있다

## section 2.4 필요한 개수만큼 인스턴스를 생성하자
```
클래스명 변수명 = new 클래스명();
```

``` 
 클래스명     변수명   =     new      클래스명();
(참조타입)  (참조변수)   (new 연산자)  (생성자)
```
#### 인스턴스를 만드는 3가지 방법
1. new 연산자와 생성자를 이용하여 만드는 방법
2. 클래스 로더를 이용하는 방법
3. 메모리에 있는 인스턴스를 복제(clone)하여 만드는 방법


## section 2.5 메소드, 메시지, 교환
객체지향의 핵심은 "메시징"
: 휼륭하고 성장 가능한 시스테을 만들기 위한 핵심은 모듈 내부의 속성과 행동이 어떤가보다 모듈이 어떻게 커뮤니케이션하는가에 달렸다
어떤객체가 다른 객체의 메소드를 호출하는 것을 메시징이라고 한다

#### 메소드 선언 방법
```java
[접근제한자] [static] 리턴type 메소드이름([매개변수,...]) {
  실행문
  ...
}
```

- 대괄호는 생략 가능
- 메소드 이름은 소문자로 시작하는 것이 관례

#### 매개변수, 전달인자 용어 구분
매개변수는 메소드의 정의 부분에 나열되어 있는 변수들을 의미하며, 전달인자는 메소드를 호출할 때 전달되는 실제 값을 의미한다

## section 2.6 메소드 선언, UML 표기법
#### 클래스 선언하고 메소드 정의해보기
<img width="500" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/4fe335ee-e482-4ce8-a7f0-cba7423d7fb4">



```java
public class MathBean {
	public void printClassName() {
		System.out.println("MathBean");
	}
	public void printNumber(int number) {
		System.out.println(number);
	}
	public int getOne() {
		return 1;
	}
	public int plus(int x, int y) {
		return x + y;
	}
}
```

#### MathBean 클래스 UML 표기법
<img width="400" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6edfd82f-49f1-4b7f-a86a-013bee555fa4">

- ```+``` : public이라는 뜻
- 리턴 타입을 뒤에 적어준다

## section 2.7 static 메소드(클래스 메소드)
> static한 메소드는 인스턴스를 생성하지 않아도 호출할 수 있다



> static 메소드는 클래스명.메소드명();으로 사용한다

```java
public class VendingMachine {
	
	public String pushProductButton(int menuId) {
		System.out.println(menuId + "를 전달받았습니다.");
		return "콜라";
	}
	
	// static 메소드
	public static void printVersion() {
		System.out.println("v1.0");
	}
}
```

```java
public class VendingMachineMain {
	public static void main(String[] args) {
		VendingMachine.printVersion(); // v1.0 출력
//		VendingMachine vm1 = new VendingMachine();
//		VendingMachine vm2 = new VendingMachine();
//		
//		String product = vm1.pushProductButton(100);
//		System.out.println(product);
	}		
}
```


## section 2.8 메소드가 실행될 때 벌어지는 일들

1. javac를 이용해 컴파일 => 소스 파일이 있던 곳에 class 파일 생성
2. ```java [파일명]``` 실행 => JVM이 **CLASSPATH**경로에서 클래스를 찾은후 읽어들인다
3. 읽어들인 클래스 정보를 PERM이라는 메모리 영역에 저장
4. JVM이 메인 메소드를 찾아 실행,
5. Java Stack이라는 메모리 영역에 메인 메소드 정보를 넣는다     
   (java stack에 저장된 메소드 실행 정보 하나를 스택 엔트리라고 한다)
6. 메인 메소드 안에 선언된 변수(로컬변수)들은 스택 엔트리에 저장된다
7. JVM이 줄을 읽어 내려가면서 (프로그램 카운터 사용) 다른 메소드를 실행하면 java stack에 스택 엔트리가 하나 더 추가된다
8. 인스턴스는 heap 메모리에 저장
9. 메소드 종료되면 java stack에서 제거된다
10. 메인 메소드가 종료되면 프로그램 종료

> 메소드 안에서 선언된 변수(=지역변수)는 메소드가 호출될 때 생겼다가, 종료될 때 사라진다


> 같은 메소드를 동시에 열 번 호출하면, 그 메소드 안의 지역 변수는 각각 다른 영역에 저장되어 사용된다


## section 2.9 private 생성자, static 메소드
> 자동생성 - 기본 생성자는 생성자가 없을 경우 컴파일할 때 자동으로 생성된다 
(생성자의 접근제한자가 private = 인스턴스 생성을 못하게 한다는 의미)







