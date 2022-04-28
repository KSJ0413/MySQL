# DTO 

DTO 

<pre>public class MemoDTO {
  private int memono ;
  private String wname ;
  private String title ;
  private String content ;  
  private String  passwd; 
  private String  viewcnt ;
  private String wdate   ;
  public MemoDTO() {
    super();
    // TODO Auto-generated constructor stub
  }
  public MemoDTO(int memono, String wname, String title, String content, String passwd, String viewcnt, String wdate) {
    super();
    this.memono = memono;
    this.wname = wname;
    this.title = title;
    this.content = content;
    this.passwd = passwd;
    this.viewcnt = viewcnt;
    this.wdate = wdate;
  }
  @Override
  public String toString() {
    return "MemoDTO [memono=" + memono + ", wname=" + wname + ", title=" + title + ", content=" + content + ", passwd="
        + passwd + ", viewcnt=" + viewcnt + ", wdate=" + wdate + "]";
  }
  public int getMemono() {
    return memono;
  }
  public void setMemono(int memono) {
    this.memono = memono;
  }
  public String getWname() {
    return wname;
  }
  public void setWname(String wname) {
    this.wname = wname;
  }
  public String getTitle() {
    return title;
  }
  public void setTitle(String title) {
    this.title = title;
  }
  public String getContent() {
    return content;
  }
  public void setContent(String content) {
    this.content = content;
  }
  public String getPasswd() {
    return passwd;
  }
  public void setPasswd(String passwd) {
    this.passwd = passwd;
  }
  public String getViewcnt() {
    return viewcnt;
  }
  public void setViewcnt(String viewcnt) {
    this.viewcnt = viewcnt;
  }
  public String getWdate() {
    return wdate;
  }
  public void setWdate(String wdate) {
    this.wdate = wdate;
  }
}

