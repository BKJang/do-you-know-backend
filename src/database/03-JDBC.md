# JDBC(Java Database Connectivity)

`JDBC`는 `JAVA`를 이용해서 데이터 베이스 접속과 SQL 실행 및 그에 따른 결과 데이터의 핸들링을 제공하는 방법과 절차에 대한 규약이다. 즉, 쉽게 말해 `JAVA`를 이용해 구현한 애플리케이션 내에서 SQL문을 실행하기 위해 만들어진 API라고 할 수 있다.

## JDBC 클래스 생성

![JDBC_CLASS_CONSTRUCTION](https://user-images.githubusercontent.com/24209005/89899209-e2c55680-dc1c-11ea-8539-ef5df37fc2bf.png)

### In Maven Dependency

```xml
<dependency>   
  <groupId>mysql</groupId>   
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.45</version>
 </dependency>
```

### Import

```java
import java.sql.*;
```

### Load Driver

```java
Class.forName( "com.mysql.jdbc.Driver" );
```

### Get Connection

```java
String dburl  = "jdbc:mysql://localhost/dbName";

Connection con =  DriverManager.getConnection ( dburl, ID, PWD );
```

```java
public static Connection getConnection() throws Exception{
	String url = "jdbc:oracle:thin:@117.16.46.111:1521:xe";
	String user = "smu";
	String password = "smu";
	Connection conn = null;
	Class.forName("oracle.jdbc.driver.OracleDriver");
	conn = DriverManager.getConnection(url, user, password);
	return conn;
}
```

### Construct Statement

```java
Statement stmt = con.createStatement();
```

### Execute Query

```java
ResultSet rs = stmt.executeQuery("select no from user" );

stmt.execute(“query”); //any SQL
stmt.executeQuery(“query”); //SELECT
stmt.executeUpdate(“query”); //INSERT, UPDATE, DELETE
```

### Get Result with ResultSet

```java
ResultSet rs = stmt.executeQuery( "select no from user" );
while (rs.next())
  System.out.println(rs.getInt( "no"));
```

### Close Connection

```java
rs.close();

stmt.close();

con.close();
```

## Example

```java
// GET
public List<GuestBookVO> getGuestBookList(){
  List<GuestBookVO> list = new ArrayList<>();
  GuestBookVO vo = null;
  Connection conn = null;
  PreparedStatement ps = null;
  ResultSet rs = null;
  try{
    conn = DBUtil.getConnection();
    String sql = "select * from guestbook";
    ps = conn.prepareStatement(sql);
    rs = ps.executeQuery();
    while(rs.next()){
      vo = new GuestBookVO();
      vo.setNo(rs.getInt(1));
      vo.setId(rs.getString(2));
      vo.setTitle(rs.getString(3));
      vo.setConetnt(rs.getString(4));
      vo.setRegDate(rs.getString(5));
      list.add(vo);
    }
  }catch(Exception e){
    e.printStackTrace();
  }finally {
    DBUtil.close(conn, ps, rs);
  }		
  return list;		
}
```

```java
// ADD
public int addGuestBook(GuestBookVO vo){
  int result = 0;
  Connection conn = null;
  PreparedStatement ps = null;
  try{
    conn = DBUtil.getConnection();
    String sql = "insert into guestbook values("
        + "guestbook_seq.nextval,?,?,?,sysdate)";
    ps = conn.prepareStatement(sql);
    ps.setString(1, vo.getId());
    ps.setString(2, vo.getTitle());
    ps.setString(3, vo.getConetnt());
    result = ps.executeUpdate();
  }catch(Exception e){
    e.printStackTrace();
  }finally {
    DBUtil.close(conn, ps);
  }
  
  return result;
}
```

```java
// CLOSE
public static void close(Connection conn, PreparedStatement ps){
  if (ps != null) {
    try {
      ps.close();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }
  if (conn != null) {
    try {
      conn.close();
    } catch (SQLException e) {
      e.printStackTrace();
    }
  }
}
```

---

#### 🙏 Reference

- [edwidth - 부스트코스 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58934/)
