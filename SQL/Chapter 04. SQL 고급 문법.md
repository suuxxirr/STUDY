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

#### 함수를 이요한 명시적인 변환
- 데이터 형식을 변환하는 함수는 CAST(), CONVERT()
- 형식만 다를뿐 동일한 기능


```sql
CAST (값 AS 데이터_형식 [(길이)]
CONVERT (값, 데이터_형식 [(길이)]
```

평균 가격 구하기 => 결과 실수
```sql
select avg(price) '평균가격' from buy;
```

 <img width="243" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/6f97cfe3-c950-4db8-9967-5d3663065d26">

 

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

<img width="422" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/65149d15-4b7b-4ec6-bc5d-a815c13a2dcf">

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

<img width="408" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/06f82f15-399b-413a-a739-713f62116d3c">


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


<img width="124" alt="image" src="https://github.com/suuxxirr/STUDY/assets/102400242/041d849a-3d8b-431e-b868-f1e91c4c76e6">

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

> left outer join 문의 읨를 왼쪽 테이블(member)의 내용은 모두 출력되어야 한다로 해석해도 좋다



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
- 




















