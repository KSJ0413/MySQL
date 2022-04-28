# DAO

<pre>
    import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;


import utility.DBClose;
import utility.DBOpen;

public class memoDAO {

  
  public boolean create(MemoDTO dto) {
  boolean flag = false;
  
  Connection con = DBOpen.getConnection();
  PreparedStatement pstmt = null;
      StringBuffer sql = new StringBuffer();
      sql.append("   insert into memo(wname, title, content, passwd, wdate)   ");
      sql.append("   values(?, ?, ?, ?, ?)  ");
      
      try {
        pstmt = con.prepareStatement(sql.toString());
        pstmt.setString(1, dto.getWname());
        pstmt.setString(2, dto.getTitle());
        pstmt.setString(3, dto.getContent());
        pstmt.setString(4, dto.getPasswd());
        pstmt.setString(5, dto.getWdate());
        
        int cnt = pstmt.executeUpdate();
        if(cnt>0); flag=true;
      } catch (SQLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }finally {
        DBClose.close(pstmt, con);
      }
      
  
  return flag;
}

public MemoDTO read(int memono) {
  MemoDTO dto = null;
  Connection con = DBOpen.getConnection();
  PreparedStatement pstmt = null;
  ResultSet rs = null;
      
  StringBuffer sql = new StringBuffer();
      sql.append("   select*   ");
      sql.append("   from memo    ");
      sql.append("   where memono = ?  ");
  
      
      try {
        pstmt = con.prepareStatement(sql.toString());
        pstmt.setInt(1, memono);
        rs = pstmt.executeQuery();
        
        
          if (rs.next()) {//데이터가 있다면
         dto = new MemoDTO();
          dto.setMemono(rs.getInt("memono"));
          dto.setWname(rs.getString("wname"));
        dto.setTitle(rs.getString("title"));
        dto.setContent(rs.getString("content"));
          dto.setPasswd(rs.getString("passwd"));
        dto.setWdate(rs.getString("wdate"));
        
        
        }
          
      } catch (SQLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }finally {
        DBClose.close(rs, pstmt, con);
      }
  return dto;
}

public boolean update(MemoDTO dto) {
 boolean flag = false;
 Connection con = DBOpen.getConnection();
 PreparedStatement pstmt = null;
     StringBuffer sql = new StringBuffer();
     sql.append("   update memo    ");
     sql.append("   set title = ?,   ");
     sql.append("   	content = ?   ");
     sql.append("       where memono = ?    ");
     
     
     
     try {
       pstmt = con.prepareStatement(sql.toString());
       pstmt.setString(1, dto.getTitle());
       pstmt.setString(2, dto.getContent());
       pstmt.setInt(3, dto.getMemono());
       
       int cnt = pstmt.executeUpdate();
       
       if(cnt>0)flag=true;
       
     } catch (SQLException e) {
       // TODO Auto-generated catch block
       e.printStackTrace();
     }finally
     {
       DBClose.close(pstmt, con);
     }
 return flag;
}
public boolean delete(int memono) {
  boolean flag = false;
  Connection con = DBOpen.getConnection();
  PreparedStatement pstmt = null;
      StringBuffer sql = new StringBuffer();
      sql.append("    delete from memo   ");
      sql.append("    where memono = ?  ");
      
      
      
      try {
        pstmt = con.prepareStatement(sql.toString());
        pstmt.setInt(1,memono);
        
        int cnt = pstmt.executeUpdate();
        if (cnt>0)flag=true;
            
      } catch (SQLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }finally {
        DBClose.close(pstmt, con);
      }
  return flag;
}

}
</pre>

