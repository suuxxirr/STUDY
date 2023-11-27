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
    - >, <. >=, = 등
      


      
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
  where height BETWEEN 163 AND 164; 
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






