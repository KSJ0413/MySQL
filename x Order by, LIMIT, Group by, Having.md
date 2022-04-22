``` 
Order by, LIMIT, Group by, Having
```

\1. ORDER BY 절
 \- Order by절은 결과의 값이나 개수에 영향을 미치지 않지만, 결과가 출력되는 순서를
  조절한다.
 \- order by 다음에 ASC(Ascending, 오름차순, 기본값) 또는 DESC(Descending, 내림차순)를 붙혀준다.

\- 데뷔일자(debut_date)가 빠른 순서대로 출력해보자

 select mem_id, mem_name, debut_date

  from member

  order by debut_date;

\- 데뷔일자(debut_date)가 늦은 순서대로 출력해보자 

 select mem_id, mem_name, debut_date

  from member

  order by debut_date **DESC**;

 \- where절과 Order by절을 함께 사용할 수 있다. order by절이 맨뒤에 온다.



 select mem_id, mem_name, height

  from member

  where height >= 164

  order by height desc;

\- 별명(alias), 컬럼순서를 order by절에 사용할 수 있다.


 select mem_id, mem_name, height 키 -- 3 키는 알리아스, 3-height

 from member        -- 1

where height >= 164    -- 2

order by 키 desc;   -- 4 order by절에 알리아스, 컬럼순서 사용가능


\- 정렬기준이 1개 열이 아니라 여러 개 열로 지정할 수 있다.
\- 우선 첫 번째 지정 열로 정렬한 후에 동일한 경우에는 다음 지정 열로 정렬할 수 있다.
\- 평균키가 큰 순서로 정렬하되, 평균 키가 같으면, 데뷔일자가 빠른 순으로 정렬한다.



 select mem_id, mem_name, height, debut_date

  from member

  where height >= 164

  order by height desc, debut_date ASC;

\- alias와 컬럼순서를 사용한 예제

select mem_id, mem_name, height, debut_date 데뷔일자 -- alias는 ""로 사용하면 인식안됨

 from member

where height >= 164

order by 3 desc, 데뷔일자;

 



\2. LIMIT

\- limit은 출력개수를 제한한다.
\- 회원테이블을 조회하는데 전체 중 앞에서 3건만 조회한다.



select *

 from member

 limit 3;

\- 데뷔날짜가 가장 빠른 3팀만 가져오기


select mem_name, debut_date

  from member

 order by debut_date

 limit 3;

\- limit 형식은 **LIMIT 시작, 개수** 이다. limit 3은 limit 0, 3과 동일하다.
 ㅇ번째부터 3건을 가져오란 의미이다.
\- 평균키(height)가 큰 순으로 정렬하여 3번째부터 2건만 조회한다.



select mem_name, height

 from member

 order by height

 limit 3, 2;



\3. DISTINCT
\- 조회된 결과에서 중복된 데이터는 1개만 가져온다.
\- 회원들의 주소는 중복될 수 있다. 

select addr from member;

\- order by 사용하기

select add from member
order by addr;

\- 주소의 중복값을 제거하기



select distinct addr from member

order by addr;



![img](http://lectureblue.pe.kr/ckstorage/images/mysql/04/2.jpg)

## 4.GROUP BY 절
\- 그룹으로 묶어주는 역할을 한다.

\- 각 회원이 구매한 물품의 개수를 구할 수 있다.
\- Group by와 집계함수가 같이 사용된다.

※ 집계함수
![img](http://lectureblue.pe.kr/ckstorage/images/mysql/04/3.jpg)

\- 우선 order by를 이용해서 회원의 amount를 확인한다.
 APN은 1+2+1+1 = 5개의 물건을 구매했다.



 select mem_id, amount

  from buy

  order by mem_id;

\- 집계함수(sum)와 Group by를 사용하면 한번에 계산된 결과를 확인할 수 있다.


select mem_id, sum(amount)

  from buy

  group by mem_id;

\- 별칭을 사용해서 확인하기



 select mem_id "회원 아이디" , sum(amount) "총 구매 개수"

  from buy

  group by mem_id;

\- 회원이 구매한 금액의 총합을 출력한다. 가격 * 수량 = 합계 



 select mem_id "회원 아이디" , sum(price * amount) "총 구매 금액"

  from buy

  group by mem_id;


\- 각 회원이 한 번 구매 시 평균 몇 개를 구매했는지 알아본다.



 select mem_id "회원 아이디" , avg(amount) "평균 구매 개수"

  from buy

  group by mem_id;

※ 참고

ROUND(숫자,반올림할 자릿수) - 숫자를 반올림할 자릿수 +1 자릿수에서 반올림

TRUNCATE(숫자,버릴 자릿수) - 숫자를 버릴 자릿수 아래로 버림(버릴 자리수 반드시 명시) 

## __중요__ 

 SELECT ROUND(3456.1234567) FROM DUAL;

-- 3456

 

SELECT ROUND(3456.1234567 ,1) FROM DUAL;

-- 3456.1

 

SELECT ROUND(3456.1234567 ,4) FROM DUAL;

-- 3456.1235

 

SELECT ROUND(3456.1234567 ,-1) FROM DUAL;

-- 3460

 

SELECT ROUND(3456.1234567 ,-2) FROM DUAL;

-- 3500

 

SELECT truncate(3456.1234567) FROM DUAL;

-- 오류

 

SELECT truncate(3456.1234567 ,1) FROM DUAL;

-- 3456.1

 

SELECT truncate(3456.1234567 ,4) FROM DUAL;

-- 3456.1234

 

SELECT truncate(3456.1234567 ,-1) FROM DUAL;

-- 3450

 

SELECT truncate(3456.1234567 ,-2) FROM DUAL;

-- 3400 




\- 회원 테이블에서 연락처가 있는 회원의 수를 카운트한다. 10명

select count(*) from member;

\- 연락처가 있는 회원만 카운트하려면 phon1 또는 phon2를 지정한다. 
 \* 표시가 아닌 열명을 지정하면 null이 아닌 값만 카운트된다. 8명

select count(phone1) "연락처가 있는 회원" from member;



\5. Having 절

\- 그룹으로 묶은 결과에서 조건을 주는 경우 사용한다.
\- 집계함수 sum()을 이용해서 회원별 총 구매 금액을 구해본다.



 select mem_id "회원 아이디" , sum(price * amount) "총 구매 금액"

  from buy

  group by mem_id;

\- 회원별 총 구매 금액이 1000 이상인 회원만을 뽑아낼때 Where를 사용하지 않고
 Having을 사용한다.
\- where절에는 집계함수를 사용할 수 없다.

\#오류난다.

 select mem_id "회원 아이디" , sum(price * amount) "총 구매 금액"

  from buy

  where sum(price * amount) > 1000

  group by mem_id;



\#Having 사용

 select mem_id "회원 아이디" , sum(price * amount) "총 구매 금액"

  from buy

  group by mem_id

  having sum(price * amount) > 1000;

\#order by사용

 select mem_id "회원 아이디" , sum(price * amount) "총 구매 금액"

  from buy

  group by mem_id

  having sum(price * amount) > 1000

  order by sum(price * amount) desc;



------------------

-- ddl
create Table test(
	toy_id   int auto_increment not null primary key,
	toy_name   char(4),
	age  int
);


-- dml
insert into est values(1, 'kim',25);
select * from test;
insert into test values(null, 'hong', 30);
insert into test (toy_id, toy_name)values(null, 'yun');

create Table test2(
	toy_id   int auto_increment not null primary key,
	toy_name   char(4),
	age  int
);

insert into test2 values(null, '보핍',25);
insert into test2 values(null, '슬랭키',80);
insert into test2 values(null, '렉스',21);

insert into test2(toy_name,age) values('홍길동',32);

select * from test2; 

alter table test2 auto_increment=100;
insert into test(toy_name, age)
values('lee',21);
insert into test2
(toy_name,age)
values('영희',23);

select * from test2;

create table test3(
toy_id int not null auto_increment primary key,
toy_name char(4),
age int
);

alter table test3 auto_increment = 1000;
set @@auto_increment_increment = 3;

insert into test3(toy_name,age) values('토마스',21);
insert into test3(toy_name,age) values('제임스',76);
insert into test3(toy_name,age) values('고든',90);

select * from test3;



__insert into test3 values(null, 'AAA',25),(null,'BBB',99),(null,'CCC',12);__

select * from test3;