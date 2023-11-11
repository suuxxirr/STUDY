# section 7. Java IO
## section 7.1 Java IO 1/5

#### IO란?
- Input & Output
- 입출력
- 입력은 키보드, 네트워크, 파일 등으로부터 받을 수 있다
- 출력은 화면, 네트워크, 파일 등에 할 수 있다

> Java IO도 객체다

- Java IO에서 사용되는 객체는 자바 세상에서 사용되는 객체
- Java IO가 제공하는 객체는 어떤 대상으로부터 읽어들여, 어떤 대상에게 쓰는 일을 한다 


> Java IO는 조립되어 사용되도록 만들어졌다
- Decorator 패턴으로 만들어졌다
=> 장식한다

<img width="600" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/248b43e6-92c8-4e48-96a1-bfcb3db43a40">        


* 초록색 마름모 : 데코레이터는 컴포넌트를 가질 수 있다 = 컴포넌트를 상속 받고 있는 것들도 가질 수 있다 

**주인공과 장식을 구분할 수 있어야 한다**
- 장식은 **InputStream, OutputStream, Reader, Writer**를 생성자에서 받아들인다
- 주인공은 어떤 대상에게서 읽어들일지, 쓸지를 결정한다
- 주인공은 1byte or byte[] 단위로 읽고 쓰는 메소드를 가진다
- 주인공은 1char or char[] 단위로 읽고 쓰는 메소드를 가진다
- 장식은 다양한 방식으로 읽고 쓰는 메소드를 가진다
- 장식은 주인공 없이 쓸 수 없다 

#### Java IO의 특수한 객체
- System.in : 표준 입력 (InputStream)
- System.out : 표준 출력 (PrintStream)
- System.err : 표준 출력 (PrintStream)


#### Java IO의 클래스 상속도
<img width="569" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/37e5feff-dece-44ac-a257-eee9ab0d3722">

<img width="677" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/72a37bff-fd60-443c-acb6-f601ae950615">


<img width="681" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/3467ea8e-18c2-4323-b31d-58132b0724ea">




#### InputStream, OutputStream
- 추상 클래스
- byte 단위 입출력 클래스는 InputStream, OutputStream의 후손이다


#### Reader, Writer
- 추상클래스
- char 단위 입출력 클래스는 Reader, Writer의 후손이다



> Java IO 객체는 InputStream, OutputStream, Reader, Writer 중 하나를 상속 받아야 한다 

> Java IO 클래스는 생성자가 중요하다
> > **장식은 InputStream, OutputStream, Reader, Writer를 생성자에서 받아들인다**



Java IO를 잘 다루려면, API는 한번쯤은 읽어보아야 한다!

***
- 키보드로 입력 받는다는 건 문자를 입력 받는 것 => char 단위 입출력         
- 한 줄 읽기: BufferReader 클래스의 readLine 메소드     
더 이상 읽어들일 것이 없으면(EOF) null을 반환       
장식!       
- 한 줄 쓰기 : PrintStream, PrintWriter      


```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class KeyboardIOExam {

	public static void main(String[] args) throws Exception {
		// 키보드로부터 한 줄씩 입력받고, 한 줄씩 화면에 출력하시오.
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		String line = null;
		while((line = br.readLine()) != null) { // 한 줄 입력 받기 
			System.out.println("읽어들인값 : "  + line);
		}
		

	}

}
```


## section 7.2 Java IO 2/5

#### File 클래스
- java.io.File 클래스는 파일의 크기, 파일의 접근 권한, 파일의 삭제, 이름 변경 등의 작업을 할 수 있는 기능을 제공하여 준다
- 여기서 주의해야 할 것은 디렉토리(폴더) 역시 파일로서 취급된다는 것이다




#### File 클래스 생성자 
<img width="678" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6adfe78f-6b65-4e9e-8fae-05ff765e7249">

- 파일 인스턴스를 만들었다고, 실제 폴더에 파일이 생성되는 건 아니다


#### File 클래스의 중요 메소드

<img width="531" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/61f4fe11-de83-4ec3-99ab-3b11da3be840">

<img width="530" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/f5a41f2a-1951-497a-8493-6500d71ece87">


***

```java
import java.io.File;
import java.io.IOException;

public class FileInfo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		if(args.length != 1) { // argument를 넘겨줌 
			System.out.println("사용법 : java FileInfo 파일이름" );
			System.exit(0);
		} // if end
		

		File f = new File(args[0]); // 파일에 대한 경로명 읽어들이기 
		if (f.exists()) { // 파일이 존재할 경우
			System.out.println("length : " + f.length());
			System.out.println("can Read : " + f.canRead());
			System.out.println("can Write : " + f.canWrite());
			System.out.println("getAbsolutePath : " + f.getAbsolutePath());
			try {
				System.out.println("getCanonicalPath : " + f.getCanonicalPath());
			} catch(IOException e ) {
				System.out.println(e);
			}
			System.out.println("getName : " + f.getName());
			System.out.println("getParent : " + f.getParent());
			System.out.println("getPath : " + f.getPath());
			
		}else { // 파일이 존재하지 않을 경우 
			System.out.println("파일이 존재하지 않습니다.");
		}
	}
}
```
argument를 넣으려면 run configuration에서 program arguments에 적어준다 

```java
import java.io.*;

// 파일 삭제 코드 
public class FileDelete {
	public static void main(String[] args) {
		if (args.length != 1) {
			System.out.println("사용법 : java FileDelete 파일이름");
			System.exit(0);
		}
		
		File f = new File(args[0]);
		if (f.exists()) {
			boolean deleteflag = f.delete();
			if(deleteflag)
				System.out.println("파일 삭제를 성공하였습니다.");
			else
				System.out.println("파일 삭제를 실패하였습니다.");
		} else {
			System.out.println("파일이 존재하지 않습니다.");
		}
	}
}
```

```java
import java.io.File;

public class FileList {

	public static void main(String[] args) {
		File file = new File("/tmp"); // tmp 폴더 안 모든 파일
		File[] files = file.listFiles(); // listFiles : 파일 배열을 리턴 
		for (int i = 0; i < files.length; i++) {
			System.out.println(files[i].getName());
		}

	}

}
```

```java
import java.io.*;

// File 클래스를 이용한 임시 파일의 생성과 삭제 
public class TempFile {

	public static void main(String[] args) {
		try {
			File f = File.createTempFile("tmp_", ".dat");
			System.out.println("60초 동안 멈춰있습니다. ");
			try {
				Thread.sleep(60000); // 60초 동안 프로그램이 멈춰있는다.
			} catch(InterruptedException e1){
				System.out.println(e1);
			}
			f.deleteOnExit(); // JVM이 종료될 때 임시파일을 자동으로 삭제한다 
		} catch (IOException e) {
			System.out.println(e);
		}

	}
}

```
*** 


#### Byte Stream
<img width="652" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/fbe080bb-e458-455a-a2bf-3a04cdc22b52">

#### InputStream, OutputStream
- 추상 클래스
- byte 단위 입출력 클래스는 InputStream, OutputStream의 후손이다

#### InputStream이 가지는 중요 메소드

<img width="730" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/c24f1b9a-ad32-4314-9f84-b0398f26abd0">

```
 ** byte 단위로 읽어들이는 read() 메소드가 int를 리턴하는 이유 **        
 => EOF를 표현하기 위해 

 int에 1byte값을 담는다 00000000 00000000 00000000 00000000        
 EOF : -1        
 1 -->  00000000 00000000 00000000 00000001       
 2의보수 --> 11111111 11111111 11111111 11111111 : -1                 
 ```
 

## section 7.3 Java IO 3/5

IO Stream : byte or char의 흐름 

#### Reader, Writer
- 추상클래스
- char 단위 입출력 클래스는 Reader, Writer의 후손이다

#### InputStrea/OutputStream Hierarchy - byte streams 
<img width="506" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/385d0d47-dedc-4f0d-bbdb-e65d924703e5">

#### Reader/Writer Hierarchy - char streams 

<img width="503" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/d4adaca2-42a0-4221-bf56-4d445880d0a8">


<img width="584" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/aa6c796a-b79d-4991-b5f7-f872f29cfe14">


<img width="607" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/83e0376f-f7e7-4f42-adbb-472656938a93">




***















