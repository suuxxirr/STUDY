# CHAPTER 05. 테이블과 뷰
## CHAPTER 05-1. 테이블 만들기

- 테이블 : 표 형태로 구성된 2차원 구조, 행과 열로 구성
- 행 : 로우 or 레코드
- 열 : 컬럼 or 필드

### GUI 환경에서 테이블 만들기
#### 데이터베이스 생성하기

1. 다음과 같이 입력하고 `execute  the selected portion or the script or everything`아이콘 클릭 
```sql
CREATE DATABASE naver_db;
```


2. sql로 만든 데이터베이스는 화면에 바로 적용되지 않기 때문에 `SCHEMAS`패널에 보이지 않는다
`SCHEMAS` 패널의 빈 곳에서 마우스 우클릭 => `Refresh all` 선택

#### 테이블 생성하기
1. 회원 테이블 생성하기

naver_db 데이터베이스의 `Tables`에서 마우스 우클릭 => `create table` 선택

<img width="393" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/c9d45cc0-2e1a-4b68-864c-9198793fac92">


2. 테이블 구성


<img width="693" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/9ae380f8-fab2-4143-b479-4f0c0d25a109">


3. `Apply` 누르면 `Apply SQL script to database` 창에서 생성된 CREATE TABLE 코드 확인 가능


<img width="585" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/0381d08a-9a8c-4cca-b3e7-36943891d4fb">




4. 같은 방식으로 구매 테이블도 생성

- GUI 에서는 기본키-외래키 관계를 선택할 수 없기 때문에 코드수정
<img width="584" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/daa2b4ae-3bab-4e97-9b28-8d569587e1e6">

#### 데이터 입력하기 

1. `SCHEMAS` - `naver_db` - `Tables` - `member` 에서 마우스 우클릭 - `select rows - limit 1000` 선택

<img width="289" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/4d026306-48f9-428a-bd1b-4e7e67abd6bf">


2. select 문 자동 생성, `result grid` 창에 결과가 보인다

3. `Insert new row` 아이콘 (result grid 위에 있음) 클릭 => 데이터 입력

4. 구매 테이블의 데이터도 입력

5. 오류 발생

- 회원 테이블과 구매 테이블은 기본 키-외래 키로 연결되어 있다
- = 구매 테이블의 mem_id 값은 반드시 회원 테이블의 mem_id로 존재해야 한다는 의미

- 회원 테이블에 아직 APN 회원을 입력하지 않음
- => cancel하고 APN 행 삭제

6. 다시 apply

### SQL로 테이블 만들기

#### 데이터베이스 생성하기 
- 다음과 같이 쿼리 창에 입력하고 스키마 패널에서 `refresh` 해준다

```sql
drop database if exists naver_db;
create database naver_db;
```
#### 테이블 생성하기 
1. create table 구문으로 회원 테이블 생성

```sql
use naver_db;
drop table if exists member;
create table member
( mem_id char(8) not null,
mem_name varchar(10) not null,
mem_number tinyint not null,
addr char(2) not null,
phone1 char(3) null,
phone2 char(8) null,
height tinyint unsigned null,
debut_date date null
);
```
2. 테이블에 기본 키 설정

- 지정할 열 뒤에 **PRIMARY KEY**문을 붙여준

```sql
use naver_db;
drop table if exists member;
create table member
( mem_id char(8) not null primary key,
mem_name varchar(10) not null,
mem_number tinyint not null,
addr char(2) not null,
phone1 char(3) null,
phone2 char(8) null,
height tinyint unsigned null,
debut_date date null
);
```

3. 구매 테이블 만들기
- **auto_increment**로 지정한 열은 **primary key**나 **unique**로 꼭 지정해주어야 한다


```sql
use naver_db;
drop table if exists buy;
create table buy
( num int auto_increment not null primary key,
mem_id char(8) not null, 
prod_name char(8) not null,
group_name char(4) null,
price int unsigned not null,
amount smallint unsigned not null,
);
```

4. 외래 키 설정

- 마지막 열의 뒤에 콤마 입력 => 외래 키와 관련된 문장 입력
- 이 테이블의 mem_id 열을 member 테이블의 mem_id 열과 외래 키 관계로 연결하라는 의미

```sql
use naver_db;
drop table if exists buy;
create table buy
( num int auto_increment not null primary key,
mem_id char(8) not null, 
prod_name char(8) not null,
group_name char(4) null,
price int unsigned not null,
amount smallint unsigned not null,
foreign key(mem_id) references member(mem_id) -- 추가
);
```


#### 데이터 입력하기 
1. 회원 테이블 데이터 입력

```sql
insert into member values('TWC', '트와이스', '9', '서울', '02', '11111111', 167, '2015-10-19');
insert into member values('BLK', '블랙핑크', '4', '경남', '055', '22222222', 163, '2016-8-8');
insert into member values('WMN', '여자친구', '6', '경기', '031', '33333333', 166, '2015-1-15');
```

2. 구매 테이블 데이터 입력

```sql
insert into buy values(null, 'BLK', '지갑', null, 30, 2);
insert into buy values(null, 'BLK', '맥북프로', '디지털', 1000, 1);
insert into buy values(null, 'APN', '아이폰', '디지털', 200, 1);
```


- GUI에서와 마찬가지로 APN은 아직 회원 테이블에 존재하지 않아 오류 발생











