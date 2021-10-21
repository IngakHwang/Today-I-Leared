# MVC (Model View Controller)

> 개발 시 화면 부분, 요청 처리 부분, 로직 처리 부분으로 나누어 개발하는 방법
>
> 웹 애플리케이션의 각 기능(클라이언트의 요청 처리, 응답 처리, 비즈니스, 로직 처리)를 분리해서 구현

![MVC흐름도](md-images/MVC%ED%9D%90%EB%A6%84%EB%8F%84.PNG)

## MVC 특징

- 각 기능이 분리되어 있어 개발 및 유지보수가 편리
- 각 기능의 재사용성이 높아짐
- 디자이너와 개발자의 작업을 분업화



## MVC 구성요소

> Controller => Servlet Class
>
> Model => DAO, VO Class
>
> View => JSP

### Controller

> 사용자의 요청 및 흐름 제어를 담당

사용자로부터 요청을 받아 어떤 비즈니스 로직을 처리해야 할지 제어

- **Servlet 클래스**가 컨트롤러의 역할 담당

- 클라이언트의 요청을 분석
- 요청에 대해서 필요한 Model을 호출
- Model에서 처리한 결과를 보여주기 위해 JSP를 선택



### Model

> 비즈니스 로직을 처리

DB 연동 같은 비즈니스 로직을 처리

- 비즈니스 로직을 수행
- 일반적으로 DAO, VO 클래스로 구성 +(Service, Manager 클래스)

#### DAO (Data Access Object)

> DB질의를 통해 Data에 접근하는 객체



#### VO (Value Object)

> DB-Table에 존재하는 Column 들을 멤버 변수로 작성하여 Column 값을 객체로 다루기 위해 사용 (Data Class)



#### Service

> DAO rapping class로 사용 => Controller => Service => DAO



#### Manager 

> 드라이버 연결과 같은 공통적으로 사용하는 클래스 묶음



### View

> 사용자에게 보여줄 화면을 담당

**JSP**가 화면 기능을 담당

Model에서 처리한 결과를 화면에 구현하여 클라이언트로 전송



## Connection Pool

> DB와 연결시킨 상태를 유지하는 기술

왜? 필요한가

​	=>DB연결에 시간이 많이 걸리는 것을 보완하기 위해서

How? 어떻게

​	=> 웹 애플리케이션이 실행됨과 동시에 연동할 DB와의 연결을 미리 설정하여 필요할 때마다 미리 연결해 놓은 상태를 이용해 빠르게 DB와 연동하여 작업



### JNDI (Java Naming and Directory Interface)

> 필요한 자원을 Key/Value 쌍으로 저장한 후 필요할 떄 키를 이용해 값을 얻는 방법

미리 접근할 자원에 키를 지정한 후 애플리케이션이 실행 중 일 때 이 키를 이용해 자원에 접근해서 작업을 하는 것

```xml
      <Context docBase="MVCBasic" path="/MVCBasic" reloadable="true" source="org.eclipse.jst.jee.server:MVCBasic">
          
      	<Resource auth="Container" driverClassName="com.mysql.cj.jdbc.Driver" maxIdle="4" maxTotal="8" name="jdbc/member" password="1234" type="javax.sql.DataSource" url="jdbc:mysql://localhost:3306/kdt13" username="root"/>
          
      </Context>
```

**속성 설명**

name : DataSource에 대한 JNDI 이름

auth : 인증 주체

driverClassName : 연결할 DB Driver Class

maxIdle : 동시에 idle 상태로 대기 할 수 있는 최대 수

maxWait : 새로 연결이 생길 때까지 기다릴 수 있는 최대 시간

user : DB 접속 ID

password : DB 접속 pw

type : DB 종류별 DataSource

url : 접속할 DB 주소, 포트번호 및 SID

### Connection(JNDI).java

```java
//DB JNDI 접근	
public static Connection getConnection() {
		Connection con = null;
		Context initCtx = null;
		
		try {
			initCtx = new InitialContext();									//JNDI에 접근하기 위해 기본 경로
			Context envCtx = (Context) initCtx.lookup("java:comp/env");		//(java:/comp/env) 지정
			DataSource ds = (DataSource)envCtx.lookup("jdbc/member");		//server.xml에 설정한 name=jdbc/membe의 DataSource 받기
			con = ds.getConnection();										//DataSource 받아 DB 연결
		} catch (NamingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return con;
	}
```



## 포워드

> 서블릿에서 다른 서블릿이나 JSP로 요청을 전달 (추가데이터포함)



### redirect

> HttpServletResponse 객체의 sendRedircet() 메서드를 이용

웹 브라우저에 재요청을 하는 방식 (서블릿 -> 브라우저 -> 서블릿)

how? => sendRedirect(url);

1. 클라이언트 웹 브라우저에서 첫 번째 서블릿 요청
2. 첫 서블릿은 sendRedirect() 메서드를 이용해 두 번째 서블릿을 웹 브라우저를 통해 요청
3. 웹 브라우저는 sendRedirect() 메서드가 지정한 두 번째 서블릿 다시 요청

```java
res.sendRedirect(url);
```



### dispatch

> RequestDispathcer 클래스의 forward() 메서드를 이용

일반적으로 포워딩 기능을 지칭

서블릿이 직접 요청하는 방법 (서블릿 -> 서블릿)

how? 

​	RequestDispatcher dis = req.getRequestDispatcher(Servlet or jsp);

​	dis.forward(req,res);

	1. 클라이언트 웹 브라우저에서 첫 서블릿 요청
 	2. 첫 서블릿은 RequestDispatcher를 이용해 두 번째 서블릿 포워드 

```java
RequestDispathcer rd = null;
rd = req.getRequestDispatcher(url);
rd.forward(req, resp);
```



## 바인딩

> 서블릿에서 다른 서블릿 or JSP로 대량의 데이터를 공유하거나 전달 을 위함

사전적의미 : 두개를 하나로 묶는다.

```java
setAttribute(String name, Object obj) //데이터를 각 객체에 바인딩 Key(name), Value(obj)로 묶기
getAttribute(String name)             //바인딩된 데이터를 key값으로 받기
removeAttribute(String name)          //바인딩된 데이터 key 값 받아 삭제
```



## MVC 연습 코드

### MemberController.java

```java
package mc.sn.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import mc.sn.service.MemberService;
import mc.sn.vo.MemberVo;

public class MemberController extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		this.doPost(req, resp);
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		// /MVCBasic/CmdController?cmd=(?)
		req.setCharacterEncoding("UTF-8");					//요청자 UTF-8 인코딩
		resp.setContentType("text/html; charset=UTF-8");	//응답자 UTF-8 인코딩
		
		String command = req.getParameter("cmd");				//queryString key=cmd의 value
		String url = "./member/result.jsp";
		MemberService service = new MemberService();
		
		RequestDispatcher rd = null;
		
		if (command.equals("list")) {							//command 값이 list면 조회
			ArrayList<MemberVo> list= service.readAll();
			req.setAttribute("key", list);
			url = "./member/member_list.jsp";
			
		} else if(command.equals("create")) {					//command == create 삽입
			String id = req.getParameter("user_id");			//요청자 중 Name=user_id 항목의 value 가져오기
			String pwd = req.getParameter("user_pwd");
			String name = req.getParameter("user_name");
			String email = req.getParameter("user_email");
			
			MemberVo vo = new MemberVo(id,pwd,name,email);
			boolean flag = service.createMember(vo);
			System.out.println(vo);
			if (flag) {
				//url = "/MVCBasic/CmdController?cmd=list";
				url = "./CmdController?cmd=list";
			} else {
				req.setAttribute("message", "input fail");
			}
		} else if(command.equals("view_input")) {				//command 
			url = "./member/member.jsp";
		} else if(command.equals("remove")) {
			String userId = req.getParameter("user_id");
			
			boolean flag = service.removeMember(userId);
			if(flag) {
				//url = "/MVCBasic/CmdController?cmd=list";
				url = "./CmdController?cmd=list";
			} else {
				req.setAttribute("message", "remove fail");
			}
			
		}
		rd = req.getRequestDispatcher(url);
		rd.forward(req, resp);
		
		//resp.sendRedirect(url);

	}
	
}
```

### MemberService.java

```java
package mc.sn.service;

import java.util.ArrayList;

import mc.sn.dao.MemberDAO;
import mc.sn.vo.MemberVo;

//필요없는 import 패키지 삭제 ctrl+shift+O

public class MemberService {
	public boolean createMember(MemberVo vo) {
		boolean flag = false;
		
		flag = new MemberDAO().insertMember(vo);
		
		return flag;
	}
	
	//전체 조회
	public ArrayList<MemberVo> readAll(){
		ArrayList<MemberVo> list = null;
		
		list = new MemberDAO().selectAll();
		
		return list;
	}
}

```

### MemberDAO.java

```java
package mc.sn.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

import mc.sn.manager.ConnectionManager;
import mc.sn.vo.MemberVo;

public class MemberDAO {
	public boolean insertMember(MemberVo vo) {
		boolean flag = false;
		//memberTBL에 삽입하는 코드
		String sql = "insert into memberTBL(member_id,member_pwd,member_name,member_email) "
				+ "values (?,?,?,?);";
		Connection con = null;
		PreparedStatement stmt = null;
		try {
			con = ConnectionManager.getConnection();
			stmt = con.prepareStatement(sql);
					
			stmt.setString(1, vo.getMemberId());
			stmt.setString(2, vo.getMemberPwd());
			stmt.setString(3, vo.getMemberName());
			stmt.setString(4, vo.getMemberEmail());
			
			int affectedCount = stmt.executeUpdate();
			if(affectedCount>0) {
				System.out.println("삽입 작업이 완료되었습니다.");
				flag = true;
			}
			stmt.close();
			con.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			ConnectionManager.closeAll(con, stmt, null);
		}
		
		return flag;
	}
	
	//전체 조회
	public ArrayList<MemberVo> selectAll(){
		ArrayList<MemberVo> list = null;
		String sql = "select * from memberTBL;";
		
		Connection con = null;
		Statement stmt = null;
		ResultSet rs = null;
		
		list = new ArrayList<MemberVo>();
		MemberVo vo = null;
		try {
			con = ConnectionManager.getConnection();
			stmt = con.createStatement();
			rs = stmt.executeQuery(sql);
			
			while(rs.next()) {
				String id = rs.getString(2);
				String pwd = rs.getString(3);
				String name = rs.getString(4);
				String email = rs.getString(5);
				
				vo = new MemberVo(id,pwd,name,email);
				vo.setJoinDate(rs.getTimestamp(6));
				list.add(vo);
			}
			
			
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return list;
	}
	
	public boolean deleteMember(String userId) {
		boolean flag = false;
		
		String sql = "delete from memberTBL where member_id=?";
		
		Connection con = null;
		PreparedStatement stmt = null;
		try {
			con = ConnectionManager.getConnection();
			stmt = con.prepareStatement(sql);
					
			stmt.setString(1,userId);
			
			int affectedCount = stmt.executeUpdate();
			if(affectedCount>0) {
				System.out.println("삭제 작업이 완료되었습니다.");
				flag = true;
			}
			stmt.close();
			con.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			ConnectionManager.closeAll(con, stmt, null);
		}
		
		return flag;
	}
}

```

### MemberVo.java

```java
package mc.sn.vo;

import java.sql.Timestamp;

public class MemberVo {
	private String memberId;
	private String memberPwd;
	private String memberName;
	private String memberEmail;
	private Timestamp joinDate;
	
	public MemberVo(String id, String pw, String name, String email) {
		this.memberId=id;
		this.memberPwd=pw;
		this.memberName=name;
		this.memberEmail=email;
	}

	public String getMemberId() {
		return memberId;
	}

	public void setMemberId(String memberId) {
		this.memberId = memberId;
	}

	public String getMemberPwd() {
		return memberPwd;
	}

	public void setMemberPwd(String memberPwd) {
		this.memberPwd = memberPwd;
	}

	public String getMemberName() {
		return memberName;
	}

	public void setMemberName(String memberName) {
		this.memberName = memberName;
	}

	public String getMemberEmail() {
		return memberEmail;
	}

	public void setMemberEmail(String memberEmail) {
		this.memberEmail = memberEmail;
	}

	public Timestamp getJoinDate() {
		return joinDate;
	}

	public void setJoinDate(Timestamp joinDate) {
		this.joinDate = joinDate;
	}

	@Override
	public String toString() {
		return "MemberVo [memberId=" + memberId + ", memberPwd=" + memberPwd + ", memberName=" + memberName
				+ ", memberEmail=" + memberEmail + "]";
	}
	
}

```

### ConnectionManager.java

```java
package mc.sn.manager;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

public class ConnectionManager {
	public static Connection getConnection2() {
		Connection con = null;
		String driver = "com.mysql.cj.jdbc.Driver";			//패키지(Driver.class 찾기)
		String url = "jdbc:mysql://localhost:3306/kdt13";	//주소(프로토콜 : @주소)
		String id = "root";									//DB 로그인 아이디
		String pwd = "1234";		
		
		try {
			Class.forName(driver);
			con = DriverManager.getConnection(url,id,pwd);
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return con;
	}
	
	public static Connection getConnection() {
		Connection con = null;
		Context initCtx = null;
		
		try {
			initCtx = new InitialContext();
			Context envCtx = (Context) initCtx.lookup("java:comp/env");
			DataSource ds = (DataSource)envCtx.lookup("jdbc/member");
			con = ds.getConnection();
		} catch (NamingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return con;
	}
	
	public static void closeConnection(Connection con) {
		if(con!=null) {
			try {
				con.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	
	public static void closeAll(Connection con, Statement stmt, ResultSet rs) {
		if (rs!=null) {
			try {
				rs.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		if(stmt!=null) {
			try {
				stmt.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		closeConnection(con);
		
	}
}

```

### member.jsp (정보입력)

```jsp
<%@page import="java.util.Date"%>
<%@page import="java.text.SimpleDateFormat"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Member 등록</title>

<link rel="stylesheet" href="../css/member.css">

</head>
<body>
<%
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	String now = sdf.format(new Date());
	
	
%>


<form action="/MVCBasic/CmdController?cmd=create" method="post">
	<table>
		<tr>
			<td>아이디</td>
			<td><input type="text" name="user_id"></td>
			<td><input type="button" name="" value="id확인"></td>
		</tr>
		
		<tr>
			<td>패스워드</td>
			<td><input type="password" name="user_pwd"></td>
			<td></td>
		</tr>
		
		<tr>
			<td>이름</td>
			<td><input type="text" name="user_name"></td>
			<td></td>
		</tr>
		
		<tr>
			<td>이메일</td>
			<td>
				<input type="text" name="user_email">			
			</td>
			<td></td>
		</tr>
		
		<tr>
			<td>가입일</td>
			<td><input type="text" name="join_date" value="<%=now %>" readonly="readonly"></td>
			<td></td>
		</tr>
		
		<tr>
			<td colspan="3" id="action_btn">
				<input type="submit" value="가입">
			</td>
		</tr>
	</table>
</form>

<a href="/MVCBasic/CmdController?cmd=list">조회 링크</a>

</body>
</html>
```

### result.jsp (정보입력확인)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Result page</title>
</head>
<body>

<h1>Result</h1>
<br>
<%
	String message = (String)request.getAttribute("message");
	out.print(message);
%>


</body>
</html>
```

### list_req.jsp (전체조회)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>리스트 요청</title>
</head>
<body>

<a href="/MVCBasic/CmdController?cmd=list">멤버 리스트 조회</a>

</body>
</html>
```

### member_list.jsp (조회확인)

```jsp
<%@page import="mc.sn.vo.MemberVo"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>멤버 리스트</title>

<link rel="stylesheet" href="../css/member.css">

</head>
<body>

<h1>멤버 리스트</h1>
<br>

<table>
	<tr><th>아이디</th><th>비밀번호</th><th>이름</th><th>이메일</th><th>가입일</th><th>삭제</th></tr>

<%
	ArrayList<MemberVo> list = (ArrayList<MemberVo>)request.getAttribute("key");
	for(MemberVo vo : list){
%>
	<tr>
		<td><%=vo.getMemberId() %></td>
		<td><%=vo.getMemberPwd() %></td>
		<td><%=vo.getMemberName() %></td>
		<td><%=vo.getMemberEmail() %></td>
		<td><%=vo.getJoinDate().toLocaleString() %></td>
		<td><a href="/MVCBasic/CmdController?cmd=remove&user_id=<%=vo.getMemberId() %>">삭제</a></td>
	</tr>
<%
	}
%>
<tr>
	<td colspan="6">
		<a href="/MVCBasic/CmdController?cmd=view_input">멤버 추가하기</a>
	</td>
</tr>
</table>

</body>
</html>
```

