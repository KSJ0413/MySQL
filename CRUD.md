``` 
MySQL
```

[01] 데이터베이스와 DBMS
\- 데이터베이스는 데이터의 집합이다. 우리 일상생활 대부분의 정보가 저장되고 관리된다
\- 데이터베이스를 관리하고 운영하는 소프트웨어를 DBMS(Database Management System) 라 한다.
\- 다양한 데이터가 저장되어 있는 데이터베이스는 여러명의 사용자나 응용 프로그램과 공유하고
 동시에 접근이 가능하다.
\- 마이크로소프트 사의 엑셀은 대용량 데이터를 관리하거나 여러 사용자와 공유하는 개념이 없어
 DBMS라고 하지 않는다.

(1) 계층형 DBMS(Hierarchical DBMS) // 잘 안쓴다

(2) 망형 DBMS (Network DBMS)// 잘 안쓴다

(3) 관계형 DBMS( Relational DBMS : RDBMS)
\- MySQL 뿐 아니라 대부분의 DBMS가 RDBMS 형태로 사용된다.
\- RDBMS의 데이터베잇이스는 테이블(table) 이라는 최소 단위로 구성되며, 이 테이블은 하나 
 이상의 열(column)과 행(row)로 이루어져 있다.
\- 워드나 엑셀의 표 모양의 2차원 구조를 테이블이다. 테이블의 형태는 아래와 같다.





\3. SQL(Structured Query Language)
\- RDBMS에서 사용되는 언어이다. '에스큐엘', '시퀄'로 읽는다.
\- SQL은 특정회사에서 만드는 것이 아니라 국제표준화기구에서 SQL에 대한 표준을 정해서
 발표하고 있다. (표준 SQL)
\- 표준 SQL을 준수하되 각 제품을 반영한 SQL을 사용하므로 SQL 언어는 DBMS 제품마다
 조금씩 다를 수 있다.

(1) SQL 언어의 종류 __외워야 한다__

 \- ANSI SQL92, 99에 기준하여 각 데이터베이스상에서 SQL을 사용할 수 있습니다. 

 \- DQL(Data Query Language), 데이터 질의어, 데이터 검색, 출력과 관련된 쿼리 
  . SELECT..FROM..WHERE 

 \- DML(Data Manapulation Language), 데이터 조작어, 데이터 입력, 수정, 삭제와 관련된 쿼리 
  . INSERT, UPDATE, DELETE 

 \- DDL(Data Definition Language), 데이터 정의어, 테이블 생성 및 삭제, 테이블 구조 수정과 관련된 쿼리 
  . CREATE TABLE, DROP TABLE, ALTER TABLE 

 \- TCL(Transaction Control Language), 트랜잭션 제어 언어, 안정적인 데이터 처리를 위한 데이터 처리와 
  관련된 명령어 
  . COMMIT, ROLLBACK, SAVEPOINT 

 \- DCL(Data Control Language)데이터 제어 언어, 권한 부여와 관련된 쿼리 
  . GRANT, REVOKE 



__4대 쿼리  DQL:SELECT,  DML: SELECT..FROM..WHERE __

### CRUD

C: SELECT

R:INSERT

U:UPDATE

D:DELETE 