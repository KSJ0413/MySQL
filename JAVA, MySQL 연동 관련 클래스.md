# JAVA, MySQL 연동 관련 클래스

 #### \1. 데이터베이스 연결 
 #### \1) 데이터베이스 드라이버 로딩, 문자열이 해당 클래스로 바뀌어 메모리에 로딩.  
  Class.forName("com.mysql.cj.jdbc.Driver"); 
####  \2) 데이터베이스 위치 지정 
  String url =  "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
---------- ----- -------
​        서버IP  포트 데이터베이스 

####  \3) DBMS Connection 연결 
  Connection con = DriverManager.getConnection(url, "javauser", "1234"); 
                          계정   패스워드

#### \2. SQL 실행 
####  \1) SQL 저장(String은 gc()를 자주 호출하게됨으로 속도가 느려짐) 
  \- String보다 StringBuffer 클래스는 속도가 빠름. 
   String sql = ""; 
   또는 
  StringBuffer sql = new StringBuffer(); 
  sql.append("");  

 #### \2) SQL 실행, 컬럼 값 
  PreparedStatement pstmt = con.prepareStatement(sql); 
  pstmt.setString(1, "JSP"); 

####  \3) INSERT, UPDATE, DELETE: 레코드 변형 
  int cnt = pstmt.executeUpdate(); 
  cnt는 실행된 레코드 수 전달 받음. 

####  \4) SELECT: 출력 
  ResultSet: SELECT 결과(레코드 집합)를 저장 
  ResultSet rs = pstmt.executeQuery(); 

####  \3. JDBC Resource관련 객체의 메모리 해제는 아래의 패턴을 사용한다.. 
  }catch(Exception e){ 
   ..... 
   ..... 
  } finally{ 
   try{ 
    if ( rs != null){ rs.close(); } 
   }catch(Exception e){} // 예외가 발생해도 무시 

   try{ 
    if ( pstmt != null){ pstmt.close(); } 
   }catch(Exception e){} 

   try{ 
    if ( con != null){ con.close(); } 
   }catch(Exception e){} 
  } 