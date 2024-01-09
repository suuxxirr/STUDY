# CHAPTER 04. SQL 고급 문법
## CHAPTER 04-1. MySQL의 데이터 형식 
### 데이터 형식
#### 정수형
- 정수형은 소수점이 없는 숫자

|데이터 형식|바이트 수|숫자 범위|
|------|---|---|
|TINYINT|1|-128~127|
|SMALLINT|2|-32.768~32.767|
|INT|4|약 -21억~ +21억|
|BIGINT|8|약 -900경 ~ + 900경|

- 입력값의 범위를 벗어나면 Out of range 오류 발생
- 범위가 0부터 시작되는 `UNSIGNED` 예약어 사용 가능

#### 문자형
- CHAR : 고정길이 문자형
- VARCHAR : 가변길이 문자형
- VARCHAR가 CHAR보다 공간을 효율적으로 운영할 수 있지만, 빠른 속도면에서는 CHAR로 설정하는 것이 좀 더 좋다

 
|데이터 형식|바이트 수|
|------|---|
|CHAR(개수)|1~255|
|VARCHAR(개수)|1~16383|


#### 대량의 데이터 형식
<img width="506" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/38b03064-b99c-4737-840c-4c267c4459f5">

- BLOB(Binary Long Object) : 글자가 아닌 이미지, 동영상 등의 데이터


#### 실수형
- 실수형은 소수점이 있는 숫자를 저장할 때 사용

|데이터 형식|바이트 수|설명|
|------|---|---|
|FLOAT|4|소수점 아래 7자리까지 표현|
|DOUBLE|8|소수점 아래 15자리까지 표현|



#### 날짜형
- 날짜형은 날짜 및 시간을 저장할 때 사용


|데이터 형식|바이트 수|설명|
|------|---|---|
|DATE|3|날짜만 저장.YYYY-MM-DD 형식으로 사용|
|TIME|3|시간만 저장. HH:MM:SS 형식으로 사용|
|DATETIME|8|날짜 및 시간을 저장. YYYY-MM-DD HH:MM:SS 형식으로 사용|

### 변수의 사용

```sql
SET @변수이름 = 변수의 값; -- 변수의 선언 및 값 대입
SELECT @변수이름; -- 변수의 값 출력
```

- 변수는 MySQL 워크벤치를 재시작할 때까지는 유지되지만, 종료하면 없어진다

```SQL
set @count = 3;
select mem_name, height from member order by height limit @ count;
```
limit에는 변수를 사용할 수 없기 때문에 문법상 오류
=> PREPARE와 EXECUTE 사용

- PREPARE는 실행하지 않고 SQL문만 준비해 놓고 EXECUTE에서 실행하는 방식


```sql
set @count = 3;
prepare mySQL from 'select mem_name, height from member order by height limit ?';

-- mySQL이라는 이름으로 준비만
-- ?는 현재는 모르지만 나중에 채워진다는 의미

execute mySQL using @count;
-- using으로 ?에 @count 변수의 값을 대입
```




### 데이터 형 변환
형 변환에는 명시적인 변환과 암시적인 변환이 있다

#### 함수를 이용한 명시적인 변환
- 데이터 형식을 변환하는 함수는 CAST(), CONVERT()
- 형식만 다를뿐 동일한 기능


```sql
CAST (값 AS 데이터_형식 [(길이)])
CONVERT (값, 데이터_형식 [(길이)])
```

평균 가격 구하기 => 결과 실수
```sql
select avg(price) '평균가격' from buy;
```

 <img width="99" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6f97cfe3-c950-4db8-9967-5d3663065d26">

 

정수로 형변환
```sql
select cast(avg(price) as signed) '평균가격' from buy;
-- 또는
select convert(avg(price), signed) '평균가격' from buy;
```

- CONCAT()함수는 문자를 이어주는 역할을 한다



```sql
select num, concat(cast(price as char), 'X', CAST(amount as char), '=') '가격X수량', price*amount '구매액'
	from buy;
```

<img width="289" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/65b2eab8-951c-4981-ae2d-1ab3d19d8b6f">

#### 암시적인 변환
- 암시적인 변환은 cast()나 convert() 함수를 사용하지 않고도 자연스럽게 형변환 되는 것을 말한다

```sql
select '100'+'200';
```
실행 결과 자동으로 숫자 100과 200으로 더해서 덧셈 수행


## CHAPTER 04-2. 두 테이블을 묶는 조인


- 조인이란 두 개의 테이블을 서로 묶어서 하나의 결과를 만들어 내는 것을 의미한다


### 내부 조인
#### 일대다 관계의 이해
- 두 테이블의 조인을 위해서는 테이블이 일대다 관계로 연결되어야 한다
- 일대다 관계란, 한쪽 테이블에는 하나의 값만 존재해야 하지만, 연결된 다른 테이블에는 여러 개의 값이 존재할 수 있는 관계

#### 내부 조인의 기본

- 일반적으로 조인이라고 부르는 것은 내부 조인을 말한다

```sql
SELECT <열 목록>
FROM <첫 번째 테이블>
  INNER JOIN <두 번째 테이블>
  ON <조인될 조건>
[WHERE 검색 조건]
```


```sql
use market_db;
select *
from buy
inner join member
on buy.mem_id = member.mem_id
where buy.mem_id='GRL';
```

<img width="922" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/65149d15-4b7b-4ec6-bc5d-a815c13a2dcf">

1. 구매 테이블의 mem_id인 'GRL' 추출
2. 'GRL' 값과 동일한 값을 회원 테이블의 mem_id 열에서 검색
3. 'GRL'이라는 아이디를 찾으면 구매 테이블과 회원 테이블의 두 행을 결합



```sql
use market_db;
select *
from buy
inner join member
on buy.mem_id = member.mem_id
```

<img width="708" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/06f82f15-399b-413a-a739-713f62116d3c">


#### 내부 조인의 간결한 표현
필요한 아이디/이름/구매 물품/주소/연락처만 추출

- 어느 테이블의 mem_id를 추출할지 정확하게 작성 => buy.mem_id


```sql
use market_db;
select buy.mem_id, prod_name, addr, concat(phone1, phone2) '연락처' 
from buy
inner join member
on buy.mem_id = member.mem_id
```


<img width="424" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/041d849a-3d8b-431e-b868-f1e91c4c76e6">

- 테이블 이름 뒤에 별칭을 주어 간결하게 표현

```sql
use market_db;
select B.mem_id, B.prod_name, B.addr, concat(B.phone1, B.phone2) '연락처' 
from buy B
inner join member M
on B.mem_id = M.mem_id
```

#### 내부 조인의 활용

- 지금까지 사용한 내부 조인은 두 테이블에 모두 있는 내용만 조인되는 방식
- => 양쪽 중에 한곳이라도 내용이 있을 때 조인하려면 외부 조인 사용


### 외부 조인
- 내부 조인은 두 테이블에 모두 데이터가 있어야 결과가 나온다
- 외부 조인은 한쪽에만 데이터가 있어도 결과가 나온다 
#### 외부 조인의 기본

```sql
SELECT <열 목록>
FROM <첫 번째 테이블>
  <LEFT | RIGHT | FULL> OUTER JOIN <두 번째 테이블 (RIGHT 테이블)>
  ON <조인될 조건>
[WHERE 검색 조건];

전체 회원의 구매 기록 출력 만들어보기


```sql
select m.mem_id, m.mem_name, b.prod_name, m.addr
from member m
	left outer join buy b -- 왼쪽에 있는 회원 테이블을 기준으로 외부 조인
    on m.mem_id = b.mem_id
order by m.mem_id;
```

> left outer join 문의 의미를 왼쪽 테이블(member)의 내용은 모두 출력되어야 한다로 해석해도 좋다



#### 외부 조인의 활용
한 번도 구매한 적이 없는 회원의 목록 추출

```sql
select distinct m.mem_id, m.mem_name, b.prod_name, m.addr
from member m
	left outer join buy b -- 왼쪽에 있는 회원 테이블을 기준으로 외부 조인
    on m.mem_id = b.mem_id
where b.prod_name is null
order by m.mem_id;
```

<img width="316" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6c4ebbe5-625d-4d76-b186-33f40ccfa63f">



### 기타 조인
기타 조인에는 상호 조인과 자체 조인이 있다

#### 상호 조인


- 상호 조인은 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 조인시키는 기능을 말한다
- => 상호 조인 결과 : 두 테이블의 각 행의 개수를 곱한 개수


#### 자체 조인
- 자체 조인은 자신이 자신과 조인한다는 의미
- 1개로 조인하면 자체 조인


## CHAPTER 04-3. SQL 프로그래밍


- **스토어드 프로시저** : MySQL에서 프로그래밍 기능이 필요할 때 사용하는 데이터베이스 개체
- SQL 프로그래밍은 기본적으로 스토어드 프로시저 안에 만들어야 한다


스토어드 프로시저의 구조 
```sql
DELIMETER $$
CREATE PROCEDURE 스토어드_프로시저_이름()
BEGIN
	이 부분에 SQL 프로그래밍 코딩
END $$ -- 스토어드 프로시저 종료
DELIMITER ; -- 종료 문자를 다시 세미콜론으로 변경
CALL 스토어드_프로시저_이름() -- 스토어드 프로시저 실행
```

### IF 문
#### IF 문의 기본 형식
```SQL
IF <조건식> THEN
	SQL문장들
END IF;
```
- 'SQL문장들'이 두 문장 이상일 때는 `BEGIN~END`로 묶어줘야 한다

예제
```sql
drop procedure if exists ifProc1; -- 기존에 ifProc1()을 만든 적이 있다면 삭제
delimiter $$
create procedure ifProc1()
begin
	if 100  = 100 then
		select '100은 100과 같습니다';
	end if;
end $$
delimiter ;
call ifProc1(); -- ifProc1() 실행
```

#### IF ~ ELSE 문


예제 
```sql
drop procedure if exists ifProc2; 
delimiter $$
create procedure ifProc2()
begin
	declare myNum int; -- declare 예약어를 사용해 myNum 변수를 선언 
    set myNum = 200; -- set 예약어로 myNum 변수에 200 대입
    if myNum = 100 then
		select '100입니다';
	end if;
end $$
delimiter ;
call ifProc1(); -- ifProc1() 실행 
```


#### IF 문의 활용
```sql
drop procedure if exists ifProc3;
delimiter $$
create procedure ifProc3()
 begin
	declare debutDate date; -- 데뷔 일자
    declare curDate date; -- 오늘
    declare days int; -- 활동한 일수
    select debut_date into debutDate -- 1
		from market_db.member
        where mem_id = 'APN';
	set curDate = current_date(); -- 2
    set days = DATEDIFF(curdate, debutdate); -- 3
    
    if (days/365) >= 5 then
		select CONCAT('데뷔한지', days, '일이나 지났습니다.');
	else
		select '데뷔한지' + days + '일밖에 안되었네요';
	end if;
end $$
delimiter ;
call ifProc3();
```
- 1 : APN의 데뷔일자(debut_date)를 추출하는 select문.  그냥 select와 달리, INTO 변수가 붙었다 => 결과를 변수에 저장
- 2 : current_date() 함수로 현재 날짜를 curDate에 저장
- 3 : datediff() 함수로 데뷔 일자부터 현재 날짜까지 일수를 days에 저장


### CASE 문
#### CASE 문의 기본 형식
```sql
CASE
	WHEN 조건1 THEN
		SQL문장들1
	WHEN 조건2 THEN
		SQL문장들2
	ELSE
		SQL문장들3
END CASE;
```
- 조건이 여러 개라면 WHEN을 여러번 반복
- 모든 조건에 해당하지 않으면 마지막 ELSE 부분 수행

예제 
```sql
drop procedure if exists caseProc;
delimiter $$
create procedure caseProc()
begin
	declare point int;
    declare credit char(1);
    set point = 88;
    
    case
		when point >= 90 then
			set credit = 'A';
		when point >= 80 then
			set credit = 'B';
		when point >= 70 then
			set credit = 'C';
		else 
			set credit = 'F';
	end case;
    select concat('취득점수==>',point), concat('학점==>',credit);
end $$
delimiter ;
call caseProc();
```

#### CASE 문의 활용
회원들의 총 구매액을 계산해서 회원의 등급을 4단계로 나누기

```sql
select m.mem_id, m.mem_name, SUM(price*amount) "총 구매액" ,
	case
		when (sum(price*amount) >= 1500) then '최우수 고객'
		when (sum(price*amount) >= 1000) then '우수 고객'
		when (sum(price*amount) >= 1) then '일반 고객'
		else '유령 고객'
    end "회원등급"
from buy b
	right outer join member m
    on b.mem_id = m.mem_id
group by m.mem_id
order by sum(price*amount) desc;
```
- order by를 사용해 총 구매액이 많은 순서로 정렬
- 구매하지 않은 회원의 아이디와 이름도 출력하기 위해, 외부 조인 사용
	- 구매한 적이 없어도 member 테이블에 있는 회원은 모두 출력해야 하므로, right outer join 사용
- case문으로 구매액에 따라 회원 등급 구분


<img width="323" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6faf243a-0a9b-4448-826a-c53bf80610d0">



### WHILE 문
#### WHILE 문의 기본 형식
- WHILE 문은 조건식이 참인 동안에 'sQL문장들'을 계속 반복한다


```sql
WHILE <조건식> DO
	SQL 문장들
END WHILE;
```
1에서 100까지의 값을 더하는 while문

```sql
drop procedure if exists whileProc;
delimiter $$
create procedure whileProc()
begin
	declare i int; -- 1에서 100까지 증가할 변수
    declare hap int;
    set i = 1;
    set hap = 0;
    
    while (i <= 100) DO
		set hap = hap + i;
        set i = i + 1;
	end while;
    select '1부터 100까지의 합 ==>', hap;
end $$
delimiter ;
call whileProc();
```
#### WHILE 문의 응용 

- `ITERATE[레이블]` : 지정한 레이블로 가서 계속 진행
- `LEAVE [레이블]` : 지정한 레이블을 빠져나간다

4의 배수 제외, 숫자 합이 1000이 넘으면 출력 후 종료
```sql
drop procedure if exists whileProc2;
delimiter $$
create procedure whileProc2()
begin
	declare i int; -- 1에서 100까지 증가할 변수
    declare hap int;
    set i = 1;
    set hap = 0;
    
    myWhile: -- while문을 mywhile이라는 레이블로 지정
    while (i <= 100) DO
		if (i%4==0) then
			set i = i + 1;
			iterate myWhile; -- 지정한 label문으로 가서 계속 진행
		end if;
		set hap = hap + i;
        if (hap > 1000) then
			leave myWhile; -- 지정한 label문 떠남
		end if;
        set i = i + 1;
	end while;
    
    select '1부터 100까지의 합 (4의배수 제외), 1000 넘으면 종료 ==>', hap;
end $$
delimiter ;
call whileProc2();

```





### 동적 SQL
- 동적 SQL을 사용하면 변경되는 내용을 실시간으로 적용시켜 사용할 수 있다

#### PREPARE와 EXECUTE
- PREPARE : SQL 문을 실행하지 않고 미리 준비만 한다
- EXECUTE : 준비한 SQL 문을 실행
- 실행한 후에는 DALLOCATE PREPARE로 문장을 해제해주는 것이 바람직



```sql
use market_db;
prepare myQuery from 'select * from member where mem_id ='blk';
execute myQuery;
deallocate prepare myQuery;
```
#### 동적 SQL의 활용

```sql
drop table if exists gate_table;
create table gate_table (id int auto_increment primary key, entry_time datetime);

set @curDate = current_timestamp(); -- 현재 날짜와 시간
prepare myQuery from 'insert into gate_table values(null, ?)';
execute myQuery using @curDate;
deallocate prepare myQuery;

select * from gate_table;
```
- 실무 예제 : 보안이 중요한 출입문에서는 출입 내역을 테이블에 기록. 출입증을 태그하는 순간의 날짜와 시간이 입력
- SQL을 실행한 시점의 날짜와 시간이 입력된다 

































