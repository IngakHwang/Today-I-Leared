# 3-Tier Programming 

## DB(MySQL)

### Table 생성

```mysql
create table memberTBL(
	member_seq		INTEGER AUTO_INCREMENT PRIMARY KEY,
	member_id		VARCHAR(10)	NOT NULL,
	member_pwd		VARCHAR(10)	NOT NULL,
	member_name		VARCHAR(10) NOT NULL,
	member_email	VARCHAR(30)	NOT NULL,
	join_date		TIMESTAMP	DEFAULT now()
);
```



## JSP 

JSP에서 DB 연결을 위한 코드

```jsp
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>	<!-- 디렉티브 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>DB Connect</title>
</head>
<body>

<%	//스크립트릿
	
	Connection con = null;
	String driver = "com.mysql.cj.jdbc.Driver";	
    //패키지(Driver.class경로)		
	String url = "jdbc:mysql://localhost:3306/kdt13";	
	//프로토콜 : 주소 : 포트 :DB이름
   
    String id = "root";
	String pwd = "1234";		
	
	Class.forName(driver);
	
	con = DriverManager.getConnection(url,id,pwd);

	if(con!=null){
		out.print("my sql connected");
		con.close();
	} else {
		out.print("fail");
	}
%>

</body>
</html>
```

