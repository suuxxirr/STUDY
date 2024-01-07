# Chapter 03. SQL 기본 문법
## Chapter 03-1. SELECT ~ FROM ~ WHERE

### 실습용 데이터베이스 구축

예제 파일 다운로드 후 오픈



<img width="412" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/507d0b72-3fd0-4670-bfa9-7328d7ed71eb">

<img width="384" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9375284f-67ea-4905-ab28-9e94d9dbe491">



```sql
DROP DATABASE IF EXISTS market_db; -- 만약 market_db가 존재하면 우선 삭제한다.
CREATE DATABASE market_db; -- 데이터베이스 새로 만들기 

USE market_db;
CREATE TABLE member -- 회원 테이블
( mem_id  		CHAR(8) NOT NULL PRIMARY KEY, -- 사용자 아이디(PK)
  mem_name    	VARCHAR(10) NOT NULL, -- 이름
  mem_number    INT NOT NULL,  -- 인원수
  addr	  		CHAR(2) NOT NULL, -- 지역(경기,서울,경남 식으로 2글자만입력)
  phone1		CHAR(3), -- 연락처의 국번(02, 031, 055 등)
  phone2		CHAR(8), -- 연락처의 나머지 전화번호(하이픈제외)
  height    	SMALLINT,  -- 평균 키
  debut_date	DATE  -- 데뷔 일자
);
CREATE TABLE buy -- 구매 테이블
(  num 		INT AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(PK)
   mem_id  	CHAR(8) NOT NULL, -- 아이디(FK)
   prod_name 	CHAR(6) NOT NULL, --  제품이름
   group_name 	CHAR(4)  , -- 분류
   price     	INT  NOT NULL, -- 가격
   amount    	SMALLINT  NOT NULL, -- 수량
   FOREIGN KEY (mem_id) REFERENCES member(mem_id)
);

INSERT INTO member VALUES('TWC', '트와이스', 9, '서울', '02', '11111111', 167, '2015.10.19');
INSERT INTO member VALUES('BLK', '블랙핑크', 4, '경남', '055', '22222222', 163, '2016.08.08');
INSERT INTO member VALUES('WMN', '여자친구', 6, '경기', '031', '33333333', 166, '2015.01.15');
INSERT INTO member VALUES('OMY', '오마이걸', 7, '서울', NULL, NULL, 160, '2015.04.21');
INSERT INTO member VALUES('GRL', '소녀시대', 8, '서울', '02', '44444444', 168, '2007.08.02');
INSERT INTO member VALUES('ITZ', '잇지', 5, '경남', NULL, NULL, 167, '2019.02.12');
INSERT INTO member VALUES('RED', '레드벨벳', 4, '경북', '054', '55555555', 161, '2014.08.01');
INSERT INTO member VALUES('APN', '에이핑크', 6, '경기', '031', '77777777', 164, '2011.02.10');
INSERT INTO member VALUES('SPC', '우주소녀', 13, '서울', '02', '88888888', 162, '2016.02.25');
INSERT INTO member VALUES('MMU', '마마무', 4, '전남', '061', '99999999', 165, '2014.06.19');

INSERT INTO buy VALUES(NULL, 'BLK', '지갑', NULL, 30, 2);
INSERT INTO buy VALUES(NULL, 'BLK', '맥북프로', '디지털', 1000, 1);
INSERT INTO buy VALUES(NULL, 'APN', '아이폰', '디지털', 200, 1);
INSERT INTO buy VALUES(NULL, 'MMU', '아이폰', '디지털', 200, 5);
INSERT INTO buy VALUES(NULL, 'BLK', '청바지', '패션', 50, 3);
INSERT INTO buy VALUES(NULL, 'MMU', '에어팟', '디지털', 80, 10);
INSERT INTO buy VALUES(NULL, 'GRL', '혼공SQL', '서적', 15, 5);
INSERT INTO buy VALUES(NULL, 'APN', '혼공SQL', '서적', 15, 2);
INSERT INTO buy VALUES(NULL, 'APN', '청바지', '패션', 50, 1);
INSERT INTO buy VALUES(NULL, 'MMU', '지갑', NULL, 30, 1);
INSERT INTO buy VALUES(NULL, 'APN', '혼공SQL', '서적', 15, 1);
INSERT INTO buy VALUES(NULL, 'MMU', '지갑', NULL, 30, 4);

SELECT * FROM member;
SELECT * FROM buy;
```


- USE문
  - market_db 데이터베이스를 선택하는 문장
- create table member (~);
  - member 테이블을 만드는 과정
- create table buy (~);
  - buy 테이블을 만드는 과정
- AUTO_INCCREMENT
    - 자동으로 숫자 입력
-  INSERT문
    - 테이블에 값 입력 
    - int형은 작은 따옴표 없이 그냥 넣어주면 된다
    - 구매 테이블의 첫 번째 열인 num은 자동으로 입력되므로 NULL로 입력한다
- `SELECT * FROM member;``SELECT * FROM buy;`
    - 입력된 내용을 확인하기 위해서 SELECT로 조회
 
### 기본 조회하기 : SELECT ~ FROM


#### USE문
- `USE 데이터베이스_이름;`

: 현재 사용하는 데이터베이스를 지정 또는 변경

#### SELECT문의 기본 형식 
```sql
SELECT select_expr
  [FROM table_references]
  [WHERE where_condition]
  [GROUP BY {col_name | expr | position}]
  [HAVING where_condition]
  [ORDER BY {col_name | expr | position}]
  [LIMIT {[offset,] row_count | row_count OFFSET offset}] -- 대괄호로 묶인 부분은 생략 가능
```

```sql
SELECT 열_이름
  FROM 테이블_이름
  WHERE 조건식
  GROUP BY 열_이름
  HAVING 조건식
  ORDER BY 열_이름
  LIMIT 숫자
```

#### SELECT와 FROM

```sql
USE market_db;
SELECT * FROM member;
```

- `SELECT` : 테이블에서 데이터를 가져올 때 사용하는 예약어
- `*` : 모든것 의미. 여기서는 테이블의 8개 열 모두 의미
- `FROM` : FROM 다음 테이블 이름
- member : 조회할 테이블 이름
- 여러 개의 열을 가져오고 싶으면 `콤마`로 구분
  

원래 테이블의 전체 이름은 `데이터베이스_이름.테이블_이름`으로 표시한다
하지만 USE문으로 데이터베이스를 지정해놓았기 때문에 다음 두 쿼리는 동일한 것이다
```sql
SELECT * FROM market_db.member;
SELECT * FROM member; 
```



### 특정한 조건만 조회하기 : SELECT ~ FROM ~ WHERE
#### 기본적인 WHERE절
- `SELECT 열_이름 FROM 테이블_이름 WHERE조건식;`

#### 관계 연산자, 논리 연산자의 사용 
- 관계 연산자
    - > , <, >=, = 등
      


      
평균키가 162 이하인 회원 검색 
```sql
select mem_id, mem_name from member where height <= 162; 
```
<img width="121" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/da0e8c61-a844-457a-b38e-a490b2c15e65">



- 논리 연산자
  - AND, OR
 

```sql
select mem_name, height, mem_number from member where height >= 164 or mem_number > 6;
```

<img width="183" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/5380d3bd-fdd3-42f6-a8ba-83d9520cb394">

#### BETWEEN ~ AND
범위에 있는 값을 구하는 경우 `BETWEEN ~ AND` 사용

```sql
select mem_name, height
  from member
  where height >= 163 AND height <= 165;
```
```sql
select mem_name, height
  from member
  where height BETWEEN 163 AND 165; 
```
두 sql문은 동일

#### IN()
```sql
SELECT mem_name, addr
  FROM member
  WHERE addr = '경기' OR addr = '전남' OR addr = '경남';
```

=> IN()을 사용하면 코드 간결하게 작성 가능


```sql
SELECT mem_name, addr
  FROM member
  WHERE addr = IN('경기', '전남', '경남');
```

#### LIKE
문자열의 일부 글자를 검색하려면 LIKE 사용
```sql
select *
  from member
  where mem_name LIKE '우%';
```
- `%` : 무엇이든 허용한다는 의미
- 앞글자가 '우'이고 그 뒤는 무엇이든 허용

```sql
select *
  from member
  where mem_name LIKE '__핑크';
```

- `_`: 한글자와 매치할 때 사용용
- 앞 두 글자는 상관없고 뒤는 '핑크'인 회원 검색 => 에이핑크, 블랙핑크 


## Chapter 03-2. 좀 더 깊게 알아보는 SELECT문
### ORDER BY
- OREDER BY 절은 결과가 출력되는 순서를 조절한다
- 기본값 : ASC (Ascending, 오름차순)
- DESC (Descending, 내림차순)


  
데뷔 일자(debut_date)가 빠른 순서대로 출력하기 
```SQL
select mem_id, mem_name, debut_date
	from member
    order by debut_date;
```
<img width="322" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/8350b088-6024-41bc-9376-e49481b543e7">



데뷔일자가 늦은 순서대로 출력하기 
```SQL
select mem_id, mem_name, debut_date
	from member
    order by debut_date desc;
```
<img width="310" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/2f46a822-2714-4d57-9d04-1be634d3eb1d">





- ORDER BY 절과 WHERE 절은 함께 사용할 수 있다
- ORDER BY 절은 WHERE절 다음에 나와야 한다 

평균 키(height)가 164 이상인 회원들을 키가 큰 순서대로 조회 


```SQL
select mem_id, mem_name, debut_date, height
	from member
    where height >= 164
    order by height desc
```

<img width="329" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/de3c0f26-b5a9-49c1-be41-7698890be00b">

- 정렬 기준을 1개 열이 아니라 여러 개 열로 지정할 수 있다
- 첫 번째 지정 열로 정렬한 후에 동일할 경우에는 다음 지정 열로 정렬

```SQL
sesect mem_id, mem_name, debut_date, height
  from member
  where height >= 164
  order by height desc, debut_date asc;
```
<img width="329" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/01b7beac-2c3e-4496-b96a-54ca19fce72a">

### LIMIT
- LIMIT는 출력하는 개수를 제한한다
- 형식 : `LIMIT 시작, 개수`
  - LIMIT 3 = LIMIT 0, 3(0번째부터 3건)
 


전체 중 앞에서 3건만 조회하기
```SQL
SELECT *
  FROM member
  LIMIT 3;
```

평균 키 순으로 정렬하되, 3번째부터 2건만 조회
```SQL
select mem_id, mem_name, height
	from member
    order by height desc
    limit 3, 2;
```


<img width="295" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/1376de6a-7edf-4d5f-a42d-a2b4fb7181a4">

### DISTINCT
- DISTINCT는 조회된 결과에서 중복된 데이터를 1개만 남긴다
- 열 이름 앞에 DISTINCT를 써주면 중복된 데이터를 1개만 남기고 제거한다 
<img width="100" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6f5fca39-67d8-47b4-b587-c8cdea6d083f">

=> 중복된 것을 눈으로 골라내기 어렵다


distinct 사용
```SQL
select distinct addr from member
```



<img width="100" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9382b581-1f70-4f34-94e1-a8a1210ec057">


### GROUP BY
- GROUP BY는 그룹으로 묶어주는 역할을 한다

```sql
select mem_id, amount from buy order by mem_id;
```
<img width="171" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/c130c0e1-3419-4ab1-9968-df5b521d6759">


APN 회원의 경우, 1+2+1+1=5개의 물건 구매 => 한 번에 구하기 위해 **집계함수** 사용


### 집계함수 
- 집계함수는 주로 GROUP BY 절과 함께 쓰이며 데이터를 그룹화해준다

<img width="554" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/14a00387-de58-4439-af6b-ec9ae20d2b57">



GROUP BY로 회원별로 묶어준 후, SUM() 함수로 구매한 개수 합치기
```sql
select mem_id, sum(amount) from buy group by mem_id;
```


<img width="283" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/0b4738b9-0915-459d-8e25-0f1e214978ca">

별칭 사용해서 보기 좋게 만들기


<img width="289" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/f085f702-a338-4958-8b65-09f170cd9679">


회원이 구매한 금액의 총합 출력하기 
```sql
select mem_id "회원 아이디", sum(amount*price) "구매 총합" from buy group by mem_id;


```
<img width="282" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/85356072-d646-4cc3-b6fc-13cf355f8f64">



전체 회원이 구매한 물품 개수의 평균 구하기

```sql
select avg(amount) "평균 구매 개수" from buy;
```


<img width="256" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/1b6e2cff-c51e-4be7-bfd9-ad6cc8d7703d">


회원 테이블에서 연락처가 있는 회원의수를 카운트하기
연락처가 있는 회원만 카운트하려면 국번(phone1) 또는 전화번호(phone2)의 열 이름을 지정해야 한다

```sql
select count(phone1) "연락처가 있는 회원" from member;
```


<img width="271" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/a9c8903b-d21b-48e2-b8fd-3887ce28da63">


### Having

- HAVING은 WHERE와 비슷한 개념으로 조건을 제한하는 것이지만, 집계 함수에 대해 조건을 제한한다
- HAIVNG 절은 꼭 GROUP BY 절 다음에 나와야 한다


총 구매액이 1000 이상인 회원만 골라내기
```sql
select mem_id "회원 아이디",sum(amount*price)"총 구매 금액" 
	from buy
	group by mem_id 
	having sum(amount*price) >1000 ;
```


<img width="289" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/b7fc41ee-3762-4dc4-831d-c6985283d4c7">




## Chapter 03-3. 데이터 변경을 위한 SQL문

### 데이터 입력 : INSERT
#### INSERT 문의 기본 문법
- INSERT는 테이블에 데이터를 삽입하는 명령어

`INSERT INTO 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...)`

- 테이블 이름 다음에 나오는 열은 생략 가능


테이블 만들어 데이터 입력
```sql
use market_db;
create table hongong1 (toy_id INT, toy_name CHAR(4), age INT);
insert into hongong1 values (1, '우디', 25);
```



#### 자동으로 증가하는 AUTO_INCREMENT
- AUTO_INCREMENT는 열을 정의할 때 1부터 증가하는 값을 입력해준다
- AUTO_INCREMENT로 지정하는 열은 꼭 PRIMARY KEY로 지정해주어야 한다


아이디 열을 자동 증가로 설정
```sql
create table hongong2(
	toy_id int auto_increment primary key,
    toy_name char(4),
    age int);
```
테이블에 데이터 입력
자동 증가하는 부분은 NULL로 채워넣으면 된다 

```sql
insert into hongong2 values (null,'보핍',25);
insert into hongong2 values (null, '슬링키', 22);
insert into hongong2 values (null, '렉스', 21);
select * from hongong2;
```

<img width="292" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/378f41aa-b17c-46b3-90c9-e9e9e3eb575b">


어느 숫자까지 증가되었는지 확인하기
```sql
select last_insert_id();
```

- AUTO_INCREMENT로 입력되는 다음 값을 100부터 시작하도록 변경하고 싶다면 다음과 같이 변경

```sql
alter table hongong2 auto_increment=100;
insert into hongong2 values(null,'재남', 35);
select * from hongong2;
```

> alter table은 테이블을 변경하라는 의미



<img width="284" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/8806457d-d03b-4301-a174-065b42e39dee">

- 처음부터 입력되는 값을 1000으로 지정하고, 다음 값은 3씩 증가하도록 설정하기
- 시스템 변수인 `@@auto_increment_incre_ment`를 변경시켜야 한다

```sql
create table hongong3 (
	toy_id int auto_increment primary key,
	toy_name char(4),
    age int);
alter table hongong3 auto_increment=1000; -- 시작값 1000으로 지정 
set @@auto_increment_increment=3; -- 증가값 3으로 지정

insert into hongong3 values (null, '토마스', 20);
insert into hongong3 values (null, '제임스', 23);
insert into hongong3 values (null, '고든', 25);
select * from hongong3;
```
<img width="286" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/8f09f043-2ebf-4cd2-8eda-2ea10d7fed97">



### 다른 테이블의 데이터를 한 번에 입력하는 INSERT INTO ~ SELECT
- 다른 테이블에 이미 데이터가 입력되어 있다면, INSERT INTO ~ SELECT 구문을 사용해 해당 테이블의 데이터를 가져와서 한 번에 입력할 수 있다


```sql
INSERT INTO 테이블_이름 (열_이름1, 열_이름2, ...)
  SELECT문;
```


1. world 데이터베이스의 city 테이블 개수 조회

```sql
SELECT * FROM world.city;
```
<img width="245" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/5c8cb6f6-b43d-4530-9c01-6443e3c98295">



2. word.city 테이블 구조 살펴보기

- DESC (describe)명령으로 테이블 구조 확인

```sql
desc world.city
```

<img width="292" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/70f5eff8-8328-4684-954b-81db506f0234">


3. LIMIT를 사용해 데이터 몇 건 살펴보기

```sql
select * from world.city limit 5;
```

4. 도시 이름과 인구 가져와 보기

```sql
create table city_popul (city_name char(35), population int);
insert into city_popul
	select name, population from world.city;
```

### 데이터 수정 : UPDATE
#### UPDATE문의 기본 문법
- UPDATE는 기존에 입력되어 있는 값을 수정하는 명령어
- 콤마(,)로 분리해서 여러 개의 열을 한 번에 변경 가능 

```sql
UPDATE 테이블_이름
  SET 열1=값1, 열2=값2, ...
  WHERE 조건;
```

방금 생성한 city_popul 테이블의 도시 이름 중에서 seoul을 서울로 변경하기



```sql
use world;
update city_popul
	set city_name='서울'
    where city_name='Seoul';
select * from city_popul where city_name='서울';
```

<img width="274" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/10d23987-3296-4ddd-9843-59d7916efc6d">



#### WHERE가 없는 UPDATE문
- UPDATE문에서 WHERE절은 문법상 생략이 가능하지만, WHERE절을 생략하면 모든 행의 값이 변경된다
- 일반적으로 전체 행의 값을 변경하는 경우는 별로 없으므로 주의하기


### 데이터 삭제 : DELETE
- DELETE를 사용해서 행 데이터를 삭제

```sql
DELETE FROM 테이블이름 WHERE 조건;
```
city_popul 테이블에서 'New'로 시작하는 도시 삭제하기

```sql
delete from city_popul
	where city_name like 'New%';
```































