# Section 0. Java 개발 시작하기

## section 0.1 자바 언어의 특징과 JDK 및 개발환경 설치

#### 자바 프로그래밍을 공부하는 이유
- 자바는 기업에서 백엔드로 가장 많이 사용하는 언어

#### 자바 언어의 특징
- 객체지향 언어
- 자바  언어는 느리지만, 버전업 되면서 다른 언어들의 장점들을 흡수  

 #### 자바 프로그램 작성과 실행
- JDK(Java Development Kit)이라는 프로그램을 다운로드하고 설치해야한다   
  ex) openjdk, oracle JDK etc...
1. 소스코드 저장할 위치에서 터미널 연 후 'code Hello.java' 실행, 아래 코드 저장  
  ```code Hello.java```   : Hello.java라는 파일 생성
```JAVA
public class Hello {
  public static void main(String[] args){
    System.out.println("Hello");
  }
}
```
> pulblic class 뒤에 오는 단어가 파일명과 같아야 한다
2. 터미널에서 ```java Hello``` 실행
3. => 터미널에 Hello 출력됨

- ```javac``` : java 컴파일러 명령  
ex) ```javac Hello.java```
- 컴파일이 성공하면 오류메세지가 없이 .class 파일이 생성된다
- ```ls -a```명령으로 파일 생성되었는지 확인
- JVM (자바 가상머신)으로 .class 파일 실행. java 명령이 JVM을 의미 이때 확장자는 입력하지 않는다  
  ex) ```java Hello```

  ## sectino 0.2 Hello의 동작 원리

#### Hello.java 파일 분석
```JAVA
public class Hello {
  public static void main(String[] args){
    System.out.println("Hello");
  }
}
```
```java
public class Hello{
....
}
```
- public class로 정의된 hello 클래스
- **public class의 클래스 이름과 파일이름은 같아야 한다** (대소문자 구분 o)

```java
public static void main(String[] args) {
....
}
```
- 클래스 안에는 **필드**와 **메소드**가 올 수 있다
- 프로그램이 실행하려면 반드시 가져야 하는 **main 메소드** (함수 아니고 메소드!)
- java로 만든 프로그램이 실행되려면 반드시 위의 코드를 가지고 있어야 한다

```java
System.out.println("Hello");
```
> java에서는 첫 번째 글자가 대문자로 시작하면 클래스
- ```System.out``` : System 클래스 가지고 있는 out이라는 의미
- ```out.println``` : out이 가지고 있는 println이라는 의미
- println 뒤에 괄호 o => println 메소드
  out 뒤에 괄호 x => out 필드
- out이 가지고 있는 println 메소드는 괄호 안의 내용을 화면에 출력

#### 컴파일하기
- javac 명령어 입력하면 파일을 읽어들여 컴파일하게 된다
- 컴파일 성공 => .class 파일 생성
  컴파일 실패 => 오류메시지
- .class 파일을 **바이트(byte) 파일**이라고 한다
- 바이트 코드 : 소스코드와 기계어의 중간 단계
> 자바의 컴파일은 기계어로 만들어주는 것이 아니라 바이트 코드로 만들어준다!

#### JVM으로 실행하기
- 자바로 작성된 프로그램은 컴파일된 클래스(=바이트파일)을 의미
- 작성된 바이트 파일을 실행하려면 JVM이 필요
  이 JVM 역할을 수행하는 것이 java 명령어


  ## section 0.3 IntelliJ를 이용한 자바프로그래밍
  
- IDE : 통합 개발 환경. 코딩, 디버그, 컴파일, 배포 등 프로그램 개발에 관련된 모든 작업을 하나의 프로그램 안에서 처리하는 환경을 제공하는 소프트웨어
- java IDE : Eclipse, IntelliJ ...

(나는 IntelliJ말고 eclipse로 진행 예정)
  









