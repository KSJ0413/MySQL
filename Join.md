# Join

\1. 내부 조인
\- 가장 많이 사용하는 조인 방법이다.
\- 두 테이블은 조인을 위해서 테이블이 일대다(one to many) 관계로 연결되어야 한다.
\- 회원테이블의 회원이 구매 테이블의 구매 정보로 연결된다. 한 회원은 여러개의 제품을 
 구매할 수 있다 



select <열 목록>

​	from <첫번째 테이블>	

​	inner hoin <두번쨰 테이불>

​	on<조인될 조건>

​	[where 검색조건]

=inner  join 은 그냥 join이라고 써도 된다

#### EX -----------------------

use market_db;

select buy.mem_id, member.mem_name, buy.prod_name, member.addr,

concat(phone1,phone2) '연락처'

 from buy

 inner join member

 on buy.mem_id = member.mem_id

 where buy.mem_id = 'GRL';



-----------------------------------------------

\2. 외부 조인
\- 내부조인은 on절의 데이터가 같은 행만 추출된다.
\- 외부조인은 on절의 데이터가 한쪽에만 있어도 결과가 나온다.
\- 전체회원의 구매 기록(구매기록이 없는 회원의 정보도 함께) 을 외부 조인으로 
 출력한다.

select <첫번째 테이블 (LEFT 테이블)>

	<LEFT : RIGHT : FULL> OUTER JOIN<두번쨰 테이블(RIGHT 테이블)>

on <조인될 조건>

[WHERE 검색 조건];

select m.mem_id, m.mem_name, b.prod_name, m.addr

 **from member m**

 **left outer join buy b**

 on m.mem_id = b.mem_id

 order by m.mem_id;

\- left outer join 문의 의미는 '왼쪽 테이블(member)의 내용은 모두 출력되어야 한다' 이다.
\- right outer join 문은 테이블의 위치만 바꾸어 주면된다.

select m.mem_id, m.mem_name, b.prod_name, m.addr

 **from buy b**

 **right outer join member m**

 on m.mem_id = b.mem_id

 order by m.mem_id;



------------------------

\- 외부 조인에서 where 문 사용하기



select m.mem_id, m.mem_name, b.prod_name, m.addr

 from member m

 left outer join buy b

 on m.mem_id = b.mem_id

 where b.prod_name is null

 order by m.mem_id;


\- full outer join은 왼쪽 외부 조인과 오른쪽 외부 조인이 합쳐진 것이다. 자주 사용되지 않는다.



\3. 기타 조인

(1) 상호조인(cross join)
\- 두 테이블의 모든행을 조인 시키는 기능이다.
\- 상호 조인 결과의 전체 행의 개수는 두 테이블의 각 행의 개수를 곱한 개수이다.
\- 상호 조인을 카티션 곱(cartesian product)라고도 부른다.

 회원과 구매 테이블의 상호 조인(cross join)은 다음과 같다.

 

 select *

  from buy

  cross join member;




(2) 상호조인(cross join) 특징
 \- ON구문 사용할 수 없다.
 \- 결과 내용에 의미가 없다. 랜덤으로 조인하기 때문이다.
 \- 상호 조인의 주 용도는 테스트하기 위해 대용량의 데이터를 생성할 때이다.

(3) 자체 조인(self join)
\- 자신과 조인한다는 의미이다.
\- 1개의 테이블을 사용한다.
\- 회사의 조직관계에서 직속 상관의 정보를 알고 싶을때 사용한다.



### EX -----------------

\- emp table 생성하여 self join확인



USE market_db;

CREATE TABLE emp_table (emp CHAR(4), manager CHAR(4), phone VARCHAR(8));

 

INSERT INTO emp_table VALUES('대표', NULL, '0000');

INSERT INTO emp_table VALUES('영업이사', '대표', '1111');

INSERT INTO emp_table VALUES('관리이사', '대표', '2222');

INSERT INTO emp_table VALUES('정보이사', '대표', '3333');

INSERT INTO emp_table VALUES('영업과장', '영업이사', '1111-1');

INSERT INTO emp_table VALUES('경리부장', '관리이사', '2222-1');

INSERT INTO emp_table VALUES('인사부장', '관리이사', '2222-2');

INSERT INTO emp_table VALUES('개발팀장', '정보이사', '3333-1');

INSERT INTO emp_table VALUES('개발주임', '정보이사', '3333-1-1');

 

SELECT A.emp "직원" , B.emp "직속상관", B.phone "직속상관연락처"

  FROM emp_table A

   INNER JOIN emp_table B

​     ON A.manager = B.emp

  WHERE A.emp = '경리부장';

 