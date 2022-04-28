# DTO+DQO=Test

<pre>
    package memo;



public class memoTest {

  public static void main(String[] args) {
   memoDAO dao = new memoDAO();
   
   //create(dao);
read(dao);
 //  update(dao);
   //delete(dao);
  }
  private static void delete(memoDAO dao) {
    int memono = 3;
    if(dao.delete(memono)) {
      p("성공");
    }else {
      p("실패");
    }
        
    
  }
  private static void update(memoDAO dao) {
MemoDTO dto = dao.read(3)    ;
dto.setTitle("12588");
  dto.setContent("1114");
  dto.setMemono(1);
  
  if(dao.update(dto)) {
    p("성공");
    dto = dao.read(3);
    p(dto);
  }else {
    p("실패");
  }
  
  
  }
  private static void read(memoDAO dao) {
    int memono = 1;
    MemoDTO dto = dao.read(memono);
    p(dto);
    
  
  }

    
</pre>

