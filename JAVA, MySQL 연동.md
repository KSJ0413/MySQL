#  JAVA, MySQL 연동

[01] JAVA, MySQL 연동 관련 클래스
 \1. 데이터베이스 연결 
 \1) 데이터베이스 드라이버 로딩, 문자열이 해당 클래스로 바뀌어 메모리에 로딩. 
  Class.forName("com.mysql.cj.jdbc.Driver"); 
 \2) 데이터베이스 위치 지정 
  String url =  "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
        ---------- ----- ------- 
        서버IP  포트 데이터베이스 

 \3) DBMS Connection 연결 
  Connection con = DriverManager.getConnection(url, "javauser", "1234"); 
                          계정   패스워드



자바는 2. SQL 실행 
 \1) SQL 저장(String은 gc()를 자주 호출하게됨으로 속도가 느려짐) 
  \- String보다 StringBuffer 클래스는 속도가 빠름. 
   String sql = ""; 
   또는 
  StringBuffer sql = new StringBuffer(); 
  sql.append("");  

\2) SQL 실행, 컬럼 값 
  PreparedStatement pstmt = con.prepareStatement(sql); 
  pstmt.setString(1, "JSP"); 

 \3) INSERT, UPDATE, DELETE: 레코드 변형 
  int cnt = pstmt.executeUpdate(); 
  cnt는 실행된 레코드 수 전달 받음. 

 \4) SELECT: 출력 
  ResultSet: SELECT 결과(레코드 집합)를 저장 
  ResultSet rs = pstmt.executeQuery(); 

 \3. JDBC Resource관련 객체의 메모리 해제는 아래의 패턴을 사용한다.. 
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



===================================================

[02] JAVA, MySQL 연동 실습 

\- lib/mysql-connector-java-8.0.27.jar를 javatest 클래스패스에 연결

\1. MySQL JDBC 연동 테스트 
\>>>>> DriverTestMySQL.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.SQLException; 

public class DriverTestMySQL { 
  public static void main(String args[]){ 
    Connection con = null;
    try{ 
      //JDBC드라이버를 로딩합니다. 
      Class.forName("com.mysql.cj.jdbc.Driver"); 
       
      //MySQL 서버를 설정합니다.                                 
      con=DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/javadb?useUnicode=true&characterEncoding=utf8","javauser","1234"); 
      System.out.println("데이터 베이스 접속이 성공했습니다."); 
    } 
    catch(SQLException ex){ 
      System.out.println("SQLException:"+ex); 
    } 
    catch(Exception ex){ 
      System.out.println("Exception:"+ex); 
    }finally{ 
      try{ 
        //데이터베이스 접속을 닫습니다. 
        if ( con != null){ con.close(); } 
      }catch(Exception e){} 
    } 
   } 
} 

\2. 실습용 테이블의 생성 
    use javadb;
    CREATE TABLE address( 
     addressnum INT        NOT NULL AUTO_INCREMENT PRIMARY KEY, -- 번호 
     name     VARCHAR(10)   NULL , -- 이름 
     handphone VARCHAR(14)   NULL , -- 핸드폰번호 
     address   VARCHAR(50)   NULL  -- 주소 
   ); 
   
  INSERT INTO address(name, handphone, address) 
  VALUES('개발자1', '000-123-1234', '대한민국'); 
  INSERT INTO address(name, handphone, address) 
  VALUES('개발자2', '111-123-1234', '일본'); 
  INSERT INTO address(name, handphone, address) 
  VALUES('개발자3', '222-123-1234', '러시아'); 
   
  SELECT addressnum, name, handphone, address 
  FROM address; 
  
  SELECT addressnum, name, handphone, address 
  FROM address WHERE addressnum=1; 
   
  UPDATE address 
  SET handphone='555-555-5555', address='터키' 
  WHERE addressnum=1; 

  DELETE FROM address WHERE addressnum=3; 
   
  SELECT addressnum, name, handphone, address 
  FROM address ORDER BY name DESC; -- 오름차순은 ASC 
  
  DELETE FROM address; 
   
\3. 레코드를 추가하는 소스 
  \- stmt.executeUpdate(sqlStr); 
\>>>>> InsertDB.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.SQLException; 
import java.sql.Statement; 
public class InsertDB { 
  public static void main(String args[]) { 
  String url = "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
    Connection con = null; 
    Statement stmt = null; 
    try { 
      Class.forName("com.mysql.cj.jdbc.Driver"); 
    } catch(Exception e) { 
      System.err.print(e.toString()); 
    } 
    try { 
      con = DriverManager.getConnection(url,"javauser","1234"); 
      stmt = con.createStatement(); 
       
      String sql = "INSERT INTO address(name,handphone,address) "; 
      sql = sql + "VALUES('개발자7', '777-777-7777', '한국')"; 
       
      //INSERT 쿼리를 먼저 실행한 후 추가된 레코드수를 리턴합니다. 
      int ret = stmt.executeUpdate(sql); 
      System.out.println("레코드 " + ret + "개가 추가되었습니다."); 
       
    } catch(Exception e) { 
      System.out.println("Exception: " + e.getMessage()); 
    } finally{ 
      try{ 
        if ( stmt != null){ stmt.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( con != null){ con.close(); } 
      }catch(Exception e){} 
    } 
  } 
} 

\4. 하나의 레코드 출력 
\>>>>> SelectDBOne.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.ResultSet; 
import java.sql.SQLException; 
import java.sql.Statement; 

public class SelectDBOne { 
  public static void main(String args[]) {
    String url = "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
    Connection con = null; 
    Statement stmt = null; 
    ResultSet rs = null; 
    try { 
      Class.forName("com.mysql.cj.jdbc.Driver"); 
    } catch(java.lang.ClassNotFoundException e) { 
      System.err.print("ClassNotFoundException: "); 
      System.err.println(e.getMessage()); 
    } 
    try { 
      con = DriverManager.getConnection(url,"javauser", "1234"); 
      stmt = con.createStatement(); 
      String sql = "SELECT addressnum, name, handphone, address "; 
      sql = sql + " FROM address WHERE addressnum=4"; 
      //쿼리를 실행하여 레코드 집합을 rs 객체로 저장합니다. 
      rs = stmt.executeQuery(sql); 
      //레코드가 있으면 true를 리턴하고 
      //첫번째 레코드로 이동 
      if(rs.next()){ 
        int addressnum = rs.getInt(1); //첫번째 정수 컬럼 
        String name = rs.getString(2); //두번째 문자열 컬럼 
        String handphone = rs.getString("handphone");//컬럼명 명시 
        String address = rs.getString("address");
        System.out.println("번호: " + addressnum); 
        System.out.println("성명: " + name); 
        System.out.println("전화번호: " + handphone); 
        System.out.println("주소: " + address); 
      } 
    } catch(SQLException e) { 
      System.out.println("SQLException: " + e.getMessage()); 
    } finally{ 
      try{ 
        if ( rs != null){ rs.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( stmt != null){ stmt.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( con != null){ con.close(); } 
      }catch(Exception e){} 
    } 
  } 

} 

\5. 전체 레코드 목록 출력 
  \- stmt = DbCon.createStatement(); 객체가 생성됩니다. 
  \- ResultSet rs = stmt.executeQuery(sqlStr);의 결과로 ResultSet객체 생성됩니다. 
\>>>>> SelectDB.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.ResultSet; 
import java.sql.SQLException; 
import java.sql.Statement; 
public class SelectDB { 
  public static void main(String args[]) { 
    String url = "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
    Connection con = null; 
    Statement stmt = null; 
    ResultSet rs = null; 
    try { 
      Class.forName("com.mysql.cj.jdbc.Driver"); 
    } catch(java.lang.ClassNotFoundException e) { 
      System.err.print("ClassNotFoundException: "); 
      System.err.println(e.getMessage()); 
    } 
    try { 
      con = DriverManager.getConnection(url,"javauser", "1234"); 
      stmt = con.createStatement(); 
      String sql = "SELECT addressnum, name, handphone, address "; 
      sql = sql + " FROM address ORDER BY name ASC"; 
      rs = stmt.executeQuery(sql); 
      while(rs.next()){ 
        int addressnum = rs.getInt(1); //첫번째 컬럼 
        String name = rs.getString(2); //두번째 컬럼 
        String handphone = rs.getString("handphone");//컬럼명 명시 
        String address = rs.getString("address"); 
         
        System.out.println("번호: " + addressnum); 
        System.out.println("성명: " + name); 
        System.out.println("전화번호: " + handphone); 
        System.out.println("주소: " + address); 
        System.out.println("-----------------------"); 
      } 
    } catch(SQLException e) { 
      System.out.println("SQLException: " + e.getMessage()); 
    } finally{ 
      try{ 
        if ( rs != null){ rs.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( stmt != null){ stmt.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( con != null){ con.close(); } 
      }catch(Exception e){} 
    } 
  } 
} 

\6. 레코드를 수정하는 소스 
\- stmt.executeUpdate(sqlStr); 
\>>>>> UpdateDB.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.SQLException; 
import java.sql.Statement; 
public class UpdateDB { 
  public static void main(String args[]) { 

​    String url = "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
​    Connection con = null; 
​    Statement stmt = null; 
​    try { 
​      Class.forName("com.mysql.cj.jdbc.Driver"); 
​    } catch(Exception e) { 
​      System.err.print(e.toString()); 
​    } 
​    try { 
​      con = DriverManager.getConnection(url,"javauser","1234"); 
​      stmt = con.createStatement(); 
​      String sql = "UPDATE address SET handphone='777-777-7777',"; 
​      sql = sql + "address='캐나다' WHERE addressnum=5"; 
​      int ret = stmt.executeUpdate(sql); 
​      System.out.println("레코드 " + ret + "개가 수정되었습니다."); 
​    } catch(SQLException e) { 
​      System.out.println("SQLException: " + e.getMessage()); 
​    } finally{ 
​      try{ 
​        if ( stmt != null){ stmt.close(); } 
​      }catch(Exception e){} 
​      try{ 
​        if ( con != null){ con.close(); } 
​      }catch(Exception e){} 
​    } 
  } 
} 

\7. 레코드를 삭제하는 소스 
\- stmt.executeUpdate(sqlStr); 
\>>>>> DeleteDB.java 
package test; 
import java.sql.Connection; 
import java.sql.DriverManager; 
import java.sql.SQLException; 
import java.sql.Statement; 
public class DeleteDB { 
  public static void main(String args[]) { 
    String url = "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf8"; 
    Connection con = null; 
    Statement stmt = null; 
    try { 
      Class.forName("com.mysql.cj.jdbc.Driver"); 
    } catch(Exception e) { 
      System.err.print(e.toString()); 
    } 
    try { 
      con = DriverManager.getConnection(url,"javauser","1234"); 
      stmt = con.createStatement(); 
       
      String sql = "DELETE FROM address "; 
      sql = sql + " WHERE addressnum=5"; 
       
      int ret = stmt.executeUpdate(sql); 
       
      System.out.println("레코드 " + ret + "개가 삭제 되었습니다."); 
       
    } catch(SQLException e) { 
      System.out.println("SQLException: " + e.getMessage()); 
    } finally{ 
      try{ 
        if ( stmt != null){ stmt.close(); } 
      }catch(Exception e){} 
      try{ 
        if ( con != null){ con.close(); } 
      }catch(Exception e){} 
       
    } 
  } 
} 