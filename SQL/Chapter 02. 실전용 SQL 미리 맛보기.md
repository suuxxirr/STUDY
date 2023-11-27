# Chapter 02. 실전용 SQL 미리 맛보기 
## Chapter 02-1. 건물을 짓기 위한 설계도 : 데이터베이스 모델링
데이터베이스 모델링 : 테이블의 구조를 미리 설계하는 것 
### 프로젝트 진행 단계
- 프로젝트 : 현실 세계에서 일어나는 업무를 컴퓨터 시스템으로 옮겨놓는 과정. 대규모 소프트웨어를 작성하기 위한 전체 과정
- 프로젝트를 진행하기 위해서 대표적으로 **폭포수 모델** 사용

- 폭포수 모델
  - 1. 프로젝트 계획
    2. 업무 분석
    3. 시스템 설계
    4. 프로그램 구현
    5. 테스트
    6. 유지 보수
   - 장점 : 각 단계가 구분되어 프로젝트의 진행 단계가 명확
   - 단점 : 문제가 발생할 겨우 앞 단계로 돌아가기 어렵다
 
### 데이터베이스 모델링
- 데이터베이스 모델링 : 우리가 살고있는 세상에서 사용되는 사물이나 작업을 DBMS의 데이터베이스 개체로 옮기기 위한 과정


### 전체 데이터베이스 구성도
- 데이터 : 하나하나의 단편적인 정보
- 테이블 : 회원이나 제품의 데이터를 입력하기 위해 표 형태로 표현한 것
- 데이터베이스 : 테이블이 저장되는 저장소
- DBMS : 데이터베이스 관리 시스템 또는 소프트웨어
  - MySQL 또한 DBMS
- 열 : 테이블의 세로
- 열 이름 : 각 열을 구분하기 위한 이름
- 데이터 형식 : 열에 저장될 데이터의 형식
- 행(행 데이터) : 실질적인 진짜 데이터
- 기본 키(주키) : 각 행을 구분하는 유일한 열
  - 중복되어서도, 비어있어서도 안된다  
  - ex) 네이버의 회원 아이디, 학번, 주민등록 번호
- SQL : 사람과 DBMS가 소통하기 위한 언어


## Chapter 02-2. 데이터베이스 시작부터 끝까지

데이터베이스 구축 절차
1. 데이터베이스 만들기
2. 테이블 만들기
3. 데이터 입력/수정/삭제하기
4. 데이터 조회/활용하기


### 데이터베이스 만들기

1. MySQL WorkBenchh 창의 [MySQL Connections] ```Local instance MySQL``` 클릭
2. 설정한 password 입력 후 ```OK```클릭
3. 좌측 하단 ```Schemas``` 누르면 MySQL에 기본적으로 들어있는 데이터베이스 3개가 보인다

> Schema와 데이터베이스는 동일한 용어
   
4. ```SCHEMAS``` 패널의 빈 부분에서 마우스 우클릭 => ```Create Schema``` 선택
5. 스키마 name 입력 후 ```Apply``` 버튼 클릭 => ```Apply SQL Script to Database```창에 SQL문 자동 생성
6. 다시 ```Apply```와 ```Finish``` 클릭 => 좌측 스키마 패널 목록에 추가된다


### 테이블 만들기 
#### 테이블 설계하기
테이블을 설계한다는 것은 테이블의 **열이름**과 **데이터 형식**을 지정하는 것

(책 71~72에 표 참고) 
- 데이터 형식
  - 문자 : CHAR
  - 숫자 : INT
  - 날짜 : DATE
 
- 열 이름을 영문으로 만들 때 띄어쓰기는 하지 않는 것이 좋다. 띄어쓰기를 할 경우에는 열 이름을 큰따옴표로 묶어야 한다
- 그래서 보통은 언더바(_)로 구분


#### 테이블 생성하기

1. ```SCHEMAS``` 패널에서 데이터베이스 왼쪽 ▶ 클릭해 확장
2. ```Table``` 우클릭 => ```Create Table``` 클릭
3. 앞에서 설계한 회원 테이블의 내용 입력
   - 테이블 이름, 열 이름은 모두 소문자로 입력
4. 완성 후 ```Apply``` 클릭   
  

<img width="842" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/65524b4f-ac2f-4536-9a52-672c4ab65330">


- CHAR(8) : 문자 8글자
- PK : 기본키
- NN : Not Null (Null 허용 안 함)



### 데이터 입력하기 


1. 스키마 패널에 생성한 데이터베이스 테이블 우클릭 후 ```Select Rows - Limits 1000``` 선택 => Result Grid 창 나타난다 (아직 데이터가 한 건도 없는 상태)

<img width="303" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/261dcc47-4bf7-4ee4-91a2-a67c20d869ae">

2. 데이터 입력 

<img width="226" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/c84b3a35-7263-473e-a9ad-793e16eecc8b">


3. 우측하단 ```Apply``` 클릭

> 기본 키로 설정한 열이 기준이 되어 오름차순으로 자동 정렬된다

#### 데이터 수정
1. 테이블 우클릭 후 ```Select Rows - Limits 1000```
2. 데이터 내용 수정
3. SQL 자동 수정

#### 데이터 삭제
1. 삭제하고자 하는 행의 제일 앞 부분 클릭 => 행이 파란색으로 선택
2. 마우스 우클릭 => ```Delete Row``` 선택
3. ```Apply``` 클릭
4. SQL 자동 수정 




### 데이터 활용하기
1. 새 SQL 입력 위해 ```Create a new SQL tab for executing queries```아이콘 클릭 (맨 왼쪽 아이콘)
2. 실행 결과를 보기 위해 ```Output```아이콘 클릭 (오른쪽 아이콘 세개 중에 가운데)
3. 작업할 데이터베이스 선택하기 위해 스키마 패널의 데이터베이스 더블 클릭 => 글자가 진하게 변한다
4. SQL 입력
5. ```Execute the selected portion of the script or everything``` 아이콘 클릭 (번개 아이콘)

 **SELECT 기본형식**

 
 ``` SELECT 열_이름 FROM 테이블_이름 [WHERE 조건]```



-  ```SELECT * FROM member;```               
: 모든 열을 보여주는 명령 




- 여러 개의 열을 콤마로 분리하면 필요한 열만 추출 

<img width="317" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/214d0b1d-a99e-4dfe-b4f3-96101f5a72cb">



- ```SELECT * FROM member WHERE member_name = '아이유';```

: WHERE 다음에 특정 조건을 입력. 회원 이름이 '아이유'인 회원만 출력되도록 한 것 




> SQL을 실행할 때 쿼리 창에 있는 모든 SQL을 수행하기 때문에, 앞으로는 필요한 부분만 마우스로 드래그해서 선택한 후에 실행 



## Chapter 02-3. 데이터베이스 개체
데이터베이스에서는 테이블 외에 인덱스, 뷰, 스토어드 프로시저, 트리거, 함수, 커서 등의 개체도 필요하다

### 인덱스 

- ```SELECT * FROM member WHERE member_name = '아이유';```

<img width="418" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/38d0a833-847a-4193-8b21-742b4365d5a1">


- `Execution Plan` 탭 클릭 

<img width="169" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/7d94b8a3-4c50-4621-8280-8888696e851e">


=> Full Table Scan (전체 테이블 검색)
: 현재 인덱스가 없기 대문에 전체 테이블을 검색해 오랜 시간이 걸려서 '아이유'를 찾은 것 

#### 인덱스 만들기
- `select * from member where member_name = '아이유'; `
- ON member(member_name) : member 테이블의 member_name 열에 인덱스를 지정

<img width="194" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/f58e3128-258e-450c-866b-2b95fbf4f16d">


=> Non-Unique Key Lookup 

### 뷰 
- 가상의 테이블
- 실제 데이터를 가지고 있지 않으며, 진짜 테이블에 링크된 개념
- 뷰의 실체 : SELECT문


1. 뷰 만들기 

```sql
create view member_view
as
select * from member;
```

2. 회원 테이블(member)이 아닌 회원 뷰(member_view)에 접근 하기

```sql
select * from member_view
```

=> 회원 테이블에 접근했을 때와 동일한 결과가 나온다 


#### 뷰 사용 이유
- 보안에 도움
- 긴 sql 문을 간략하게 만들기 가능
- 자세한 내용은 5장에서


### 스토어드 프로시저(stored procedure)

- MySQL에서 제공하는 프로그래밍 기능
- 스토어드 프로시저를 통해 MySQL에서도 기본적인 형태의 일반 프로그래밍 로직 코딩 가능


1. 
```sql
select * from member where member_name = '나훈아';
select * from product where product_name = '삼각김밥'; 
```
- 다음 두 sql 입력 후 한꺼번에 실행  => 별도의 탭으로 동시에 결과가 나온다
- 매번 두 줄의 sql을 입력해야 한다면 불편


<img width="151" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/4ed928cb-0cb9-4a2a-afd7-87a2c7591382">

2. 두 sql을 하나의 스토어드 프로시저로 만들기

```sql
delimiter //
create procedure myProc()
begin
	select * from member where member_name = '나훈아';
	select * from product where product_name = '삼각김밥'; 
end //
delimiter ;

```
- `delimiter // ~delimiter ;` : 구분 문자
- `begin`과 `end` 사이에 sql문을 넣는다


3.

- `call myProc()` 
=> 두 sql을 실행한 것과 동일한 결과 















