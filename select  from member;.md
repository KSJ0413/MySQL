select * from member;
select addr, debut_date, mem_name from member;
select addr 주소,
debut_date "데뷔 일자",
mem_name 이름
from member;
select * from member where addr = '서울';
select * from member where mem_name = '블랙핑크';

select mem_id, mem_name from member where height<= 162;

select mem_id, mem_name, height
  from member
  where height <= 165
    and mem_number > 6;
select mem_id, mem_name, height from member where height <= 165 or mem_number > 6;

select mem_name, height
from member
where height between 163 and   165;

select mem_name, addr from member where addr = '경기' or addr = '전남';
select mem_name, addr from member where addr in ('경기','전남','경남');

 select*
 from member
 where mem_name like '%핑%';
 -- where mem_name = '우';

 select *
 from member
 where mem_name like '__핑크';

 select height
 from member
 where mem_name like '에이핑크';

 select mem_name, height
 from member 
 where height > height '에이핑크';

 select mem_name, height
 from member 
 where height >(
 select height
 from member
 where mem_name = '에이핑크');

 select addr
 from member
 where mem_name = '에이핑크';

 select*
 from member
 where addr =( select addr
 from member
 where mem_name = '에이핑크');

 select mem_number
 from member
 where mem_name = '에이핑크';

 select *
 from member
 where mem_number =( select mem_number
 from member
 where mem_name = '에이핑크');

 select *
 from member
 where mem_number = 6
 and  addr = '경기';

 select mem_id
 from member
 where mem_name in ( select mem_name

 from member
 where mem_number = 6
 and  addr = '경기');



--------------------------

select mem_id, mem_name, debut_date
from member
order by debut_date desc;

select mem_id , mem_name , height 키		-- 3
from member								-- 1
where height >= 164						-- 2
order by 3 desc;						-- 4

select mem_id, mem_name, height, debut_date 일자 -- alias는 ""로 사용하면 인식안됨
from member
where height >= 164
order by 3 desc, 일자;

select mem_name, debut_date
from member
order by debut_date
limit 3;

select mem_name, height
from member
order by height desc
limit 3,2;

select distinct addr from member;

select mem_id "회원 아이디", sum(amount) "총 구매 갯수"
from buy
group by  mem_id;

select mem_id "회원 아이디", sum(price * amount) "총 구매 금액"
from buy
group  by mem_id;

select mem_id "회원 아이디", avg(amount) "평균 구매 개수"
from buy
group  by mem_id;

select mem_id "회원 아이디", round(avg(amount)) "평균 구매 개수"
from buy
group  by mem_id;

select round(346.184848,3) ;

select truncate(3456.123456789,3) from dual;

select count(*) from member;
select count(phone1) "연락처가 있는 회원" from member;

select * from member
where phone1  is null;

select mem_id "회원 아이디", sum(price * amount) "총 구매 금액"
from buy
group by mem_id
having sum(price*amount)>1000
order by '총 구매 금액';