# section 9. 스레드(Thread)와 네트워크 프로그래밍
## section 9.1 스레드(Thread) 개념, 실행방법 

### 병렬 vs 병행
- 병행(Concurrent)은 멀티스레드 프로그래밍을 의미
- 병렬(Parallel)은 멀티 코어 프로그래밍(cpu 여러개를 동시에 사용)을 의미
- 여기서 살펴볼 것은 병행 프로그래밍(동시성 프로그래밍, 멀티스레드 프로그래밍)

<img width="357" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/5d578025-d47b-4722-ad13-a79d95182dae">


### Process
- 각각의 프로세스는 메모리 공간에서 **독립적으로 존재**
- 각각의 프로세스는 다음 그림과 같이 자신만의 메모리 구조를 가진다
- 프로세스 A,B,C가 있을 경우 각각 프로세스는 모두 같은 구조의 메모리 공간을 가진다
- 독립적인만큼 다른 프로세스의 메모리 공간에 접근할 수 없다

<img width="358" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/ad3c7676-949b-43d9-92a5-e28fe19019fb">

### IPC
- 프로세스 A에서 프로세스 B를 직접 접근할 수 없기 때문에, 프로세스 간의 통신을 하는 특별한 방식이 필요
- 메일슬록(mailslot), 파이프(pipe) 등이 **프로세스 간의 통신 즉, IPC**의 예
- 프로세스는 독립적인 메모리 공간을 지니기 때문에 IP를 통하지 않고 통신할 수 없다
- 프로세스가 여럿이 병렬적으로 실행되기 위해서는 필연적으로 컨텍스트 스위칭이 발생. 이것을 해결할 수 있는 것이 **Thread**




### Thread
- 스레드는 하나의 프로그램 내에 존재하는 여러 개의 실행 흐름을 위한 모델
- 우리가 생각하는 프로그램이 실행되기 위해서 하나의 실행흐름으로 처리할 수도 있지만, 다수의 실행 흐름으로 처리할 수도 있다
- 다음 그림과 같이 스레드는 프로세스와 별개가 아닌 프로세스를 구성하고 실행하는 흐름


<img width="298" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/78d8012d-8b72-40f9-b1c6-e4b9daf648ea">

### 스레드 vs 프로세스
- **스레드는 프로세스 안에 존재하는 실행 흐름**
- 스레드는 프로세스의 heap, static, code 영역 등을 공유
- 스레드는 stack 영역을 제외한 메모리 영역 공유
- 스레드가 code 영역을 공유하기 때문에, 프로세스 내부의 스레드들은 프로세스가 가지고 있는 함수를 자연스럽게 모두 호출 가능
- 스레드는 IPC없이도 스레드 간의 통신 가능
- A, B 스레드는 통신하기 위해 heap 영역에 메모리 공간을 할당, 두 스레드가 자유롭게 접근 가능


<img width="480" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/a7fe6b93-317a-4738-9661-f44cee2cca28">

메모리 공간에서의 스레드 

### 멀티스레드 실행방식 
메인 메소드가 실행되면 메인 스레드가 무조건 하나 만들어진다 

<img width="530" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/1b2dd5be-6cf4-4470-ab47-431df2f5cbbb">

### Thread 클래스를 상속받아 스레드 작성하기 


<img width="152" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/5acf623d-cf14-40ca-a091-001aba5ba659">

> thread의 run() 메소드를 반드시 상속받고 있는 클래스에서 오버라이딩해주어야 한다


```java
Class Xxx extends Thread {
  public vid run() {
    // 동시에 실행될 코드 작성
  }
}
```
```java
Xxx x = new Xxx();
x.start():
```


***
```java
public class MyThreadExam {

	public static void main(String[] args) {
		// 1초마다 * 를 10번 출력하는 프로그램을 작성하시오 
		for (int i = 0; i < 10; i++) {
			System.out.print("*");
			try {
				Thread.sleep(1000); // 1초마다 반복 => 1초간 쉰다
			}catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
		
		// 1초마다 + 를 10번 출력하는 프로그램을 작성하시오 
		for (int i = 0; i < 10; i++) {
			System.out.print("+");
			try {
				Thread.sleep(1000); // 1초마다 반복 => 1초간 쉰다
			}catch(InterruptedException e) {
				e.printStackTrace();
			}
		}

	}

}

```
*과 +를 동시에 출력하고 싶다면? 
```java
package com.example.thread;

// 1. Thread 클래스를 상속받는다 
public class MyThread extends Thread{
	private String str;
	
	public MyThread(String str) {
		this.str = str;
	}
	
	// 2. run() 메소드를 오버라이딩한다
	// 동시에 실행시키고 싶은 코드를 작성한다 
	
	@Override
	public void run() {
		for(int i = 0; i < 10; i++) {
			System.out.print(str);
			try {
				Thread.sleep(1000); // 1초마다 반복 => 1초간 쉰다
			}catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```

```java
package com.example.thread;

public class MyThreadExam {

	public static void main(String[] args) {
		String name = Thread.currentThread().getName();
		System.out.println("thread name : " + name);
		System.out.println("start");
		
		MyThread mt1 = new MyThread("*");
		MyThread mt2 = new MyThread("+");
		
		// 3. thread는 start() 메소드로 실행한다 
		mt1.start(); // start에서 새로운 흐름 발생 
		mt2.start();
		
		System.out.println("end!");
		
		/*
		 * main 메소드 실행되면서 main thread 실행
		 * main thread 한 줄씩 실행되다
		 * start()를 만나면 mt1에 해당되는 thread 발생, run() 메소드 실행
		 * run() 실행되는 도중에 main thread는 바로 다음줄 실행
		 * mt2에 해당되는 thread 발생, run() 메소드 실행 
		 * ==> 흐름 3개 
		 */

	}

}
```

### Runnable 인터페이스를 구현하여 스레드 작성하기 
<img width="312" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/2dcabb0d-ee0c-42b5-bd57-2229c73124f2">

```java
class Xxx implements Runnable{
  public void run(){
    // 동시에 실행될 코드 작성
  }
}
```
```java
Xxx x = new Xxx();
Thread t = new Thread(x);
t.start();
```

***
```java
package com.example.thread;

// 1. Runnable 인터페이스를 구현한다 
public class MyRunnable implements Runnable {
	private String str;
	
	public MyRunnable(String str) {
		this.str = str;
	}
	
	// 2. run() 메소드를 오버라이딩한다 
	@Override
	public void run() {
		for(int i = 0; i < 10; i++) {
			System.out.print(str);
			try {
				Thread.sleep(1000); // 1초마다 반복 => 1초간 쉰다
			}catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```
```java
package com.example.thread;

public class MyThreadExam2 {

	public static void main(String[] args) {
		String name = Thread.currentThread().getName();
		System.out.println("thread name : " + name);
		System.out.println("start");
		
		MyRunnable mr1 = new MyRunnable("*");
		MyRunnable mr2 = new MyRunnable("*");
		
		// 3. Thread 인스턴스를 생성, 생성자에 Runnable 인스턴스를 넣엉준다 
		Thread t1 = new Thread(mr1);
		Thread t2 = new Thread(mr2);
		
		// 4. Thread가 가지고 있는 start() 메소드를 호출한다
		t1.start();
		t2.start();
		
		System.out.println("end!");
		

	}

}
```

## section 9.2 네트워크 프로그래밍 1/2

### IP 주소와 Port
- 컴퓨터를 구분하는 주소 : IP
- 컴퓨터 안에 있는 서버들을 구분하는 값 : Port


<img width="460" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9fafdf31-93ed-408c-a160-c28947dad35d">


- 127.0.0.1 : 컴퓨터 자신의 IP


### 도메인 주소
- https://www.naver.com
- https://ww.daum.net
- https://www.google.com
- localhost : 컴퓨터 자신의 도메인 

### 도메인 네임 서버 (Domain Name Server: DNS)
- 도메인 주소를 IP로 변환

### IP 주소 알아내기
- ```InetAddress```로 알아낸다
- 사용자 컴퓨터의 IP주소 알아내기

```java
InetAdderss ia = InetAddress.getLocalHost();
Sytem.out.println(ia.getHostAddress());
```

### Client & Server 프로그래밍
- Socket : Server에 접속을 하는 역할
- ServerSocket : Client의 접속 요청을 기다리는 역할 
  - Client 요청을 기다리다가 접속하면 Socket 반환
 
- Socket과 Socket간에는 IO 객체를 이용하여 통신 가능


 

#### 브라우저(client) 요청 결과를 출력하는 Server 프로그램 작성하기
- http://ip:port 주소로 브라우저는 요청을 보낼 수 있다
- ServerSocket은 특정 port 접속 요청을 기다릴 수 있다
- 브라우저는 서버와 연결이 되면 요청 정보 전송
- 서버는 브라우저가 보내는 요청 정보를 읽어들인 후, 그 결과를 출력할 수 있다



```java
import java.net.ServerSocket;
import java.net.Socket;

public class VerySimpleWebServer {

	public static void main(String[] args) throws Exception{
		// 9090 port로 대기한다
		ServerSocket ss = new ServerSocket(9090);
		
		// 클라이언트를 기다린다 
		// 클라이언트가 접속하는 순간, 클라이언트와 통신할 수 있는 socket을 반환
		// 브라우저에서 http://127.0.0.1:9090/ 을 입력
		
		// socket은 브라우저(client)와 통신할 수 있는 객체 
		Socket socket = ss.accept();
		ss.close();

	}

}
```


## section 9.3 네트워크 프로그래밍 2/2
### 웹 서버의 동작
- 동시에 여러번 동작한다
  - 클라이언트가 접속할 때까지 대기
  - 클라이언트가 접속하면 연결이 된다
  - 클라이언트가 보내주는 정보를 읽어들인다 (빈줄까지!)
      - 첫줄(GET/)
      - 헤더들 (여러줄) 헤더명:헤더값
      - 빈줄
  - 서버는 응답을
      - 첫줄(200 OK)
      - 헤더들(Body의 크기)
      - 빈줄
      - Body 내용이 전달된다
  - 연결이 끊어진다
 

```java
public class WebServer {

	public static void main(String[] args) throws Exception{
		// 클라이언트가 접속할 때까지 대기할 때 필요한 객체가 ServerSocket
		ServerSocket serverSocket = new ServerSocket(10000);
		Socket clientSocket = serverSocket.accept(); // 대기한다. 클라이언트가 접속하면 클라이언트와 통신하는 Socket이 반환된다 
		
		InputStream inputStream = clientSocket.getInputStream();
		BufferedReader br = new BufferedReader(new InputStreamReader(inputStream));
		
		// http://localhost:10000/hello
		// GET /hello HTTP/1.1
		System.out.println(br.readLine());
		
		String line = null;
		line = br.readLine();
		while(!(line = br.readLine()).equals("")){
			System.out.println(line); 
		} // 빈줄까지 읽어들이면 끝
		
		// 응답하기
		OutputStream outputStream = clientSocket.getOutputStream();
		PrintWriter pw = new PrintWriter(new OutputStreamWriter(outputStream));
		
		pw.println("HTTTP/1.1 200 OK"); // 응답
		pw.println("name:kim");
		pw.println("email:urstory@gmail.com");
		pw.println("<html>"); // body 
		pw.println("hello world");
		pw.println("</html");
		
		pw.flush();
		br.close();
		pw.close();
		clientSocket.close();
		serverSocket.close();
		
	}

}
```






