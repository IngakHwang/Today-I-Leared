# Servlet 

> 서버 쪽에서 실행되면서 클라이언트의 요청에 따라 동적으로 ㅅ버ㅣ스를 제공하는 자바 클래스
>
> 톰갯과 같은 JSP/Servlet컨테이너에서 실행



## Today Learn Tip

이클립스에서 DB .jar을 넣기 위해선 [프로젝트파일 - src -  main - webapp - WEB-INF - lib] 경로에 .jar 파일 넣기

WEB-INF에 있는 web.xml 파일은 servlet 클래스와 url을 연결하기 위한 파일



## member.jsp

> 사용자에게 아이디,비밀번호,이름, 이메일을 입력 받아 DB에 넣기 (DB는 이미 만들어져 있음)

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


<form action="/MVCBasicInstructor/CmdController2" method="post">
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

</body>
</html>
```

## web.xml 

> Servlet과 연결을 위함

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>MVCBasic</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
    
  <servlet>
  	<servlet-name>controller</servlet-name>								//매핑 이름
  	<servlet-class>mc.sn.controller.MemberController</servlet-class>	//패키지 클래스명
  </servlet>
  <servlet-mapping>
  	<servlet-name>controller</servlet-name>								//매핑 이름
  	<url-pattern>/CmdController2</url-pattern>							//주소
  </servlet-mapping>			// 매핑 이름이 같은 servlet-class와 url을 연결
</web-app>

```

##  MemberVO.java

> Data Class

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

## MemberService.java

> DB로 Insert Query 넣는 로직

```java
package mc.sn.service;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import mc.sn.vo.MemberVO;

public class MemberService {
	public boolean createMember(MemberVO vo){
		boolean flag = false;
		
		String sql = "insert into memberTBL(member_id,member_pwd,member_name,member_email)"+
				"values (?,?,?,?)";
		try {
			Connection con = this.makeConnection();
			PreparedStatement stmt = con.prepareStatement(sql);
			
			stmt.setString(1,vo.getMemberId());
			stmt.setString(2,vo.getMemberPwd());
			stmt.setString(3,vo.getMemberName());
			stmt.setString(4,vo.getMemberEmail());
			
			int affectedCount = stmt.executeUpdate();
			if (affectedCount > 0) {
				System.out.println("삽입 완료");
				return flag=true;
			}
			stmt.close();
			con.close();
		}catch(SQLException e) {
			e.printStackTrace();
		}catch(ClassNotFoundException e) {
			e.printStackTrace();
		}

		return flag;
	}
	
	public Connection makeConnection() throws ClassNotFoundException, SQLException {
		Connection con = null;
		
		String driver = "com.mysql.cj.jdbc.Driver";			//패키지(Driver.class 찾기)
		String url = "jdbc:mysql://localhost:3306/kdt13";	//주소(프로토콜 : @주소)
		String id = "root";									//DB 로그인 아이디
		String pwd = "1234";		
		
		Class.forName(driver);
		
		con = DriverManager.getConnection(url,id,pwd);
		
		return con;
	}
}

```

## MemberController.java

> HttpServlet 입력처리

```java
package mc.sn.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import mc.sn.service.MemberService;
import mc.sn.vo.MemberVO;

public class MemberController extends HttpServlet {
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		this.doPost(req, resp);
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		req.setCharacterEncoding("UTF-8");					//요청자 UTF-8 인코딩
		resp.setContentType("text/html; charset=UTF-8");	//응답자 UTF-8 인코딩
		PrintWriter out = resp.getWriter();					//응답스트립 객체
		
		String id = req.getParameter("user_id");			//요청자 중 Name=user_id 항목의 value 가져오기
		String pwd = req.getParameter("user_pwd");
		String name = req.getParameter("user_name");
		String email = req.getParameter("user_email");
		
		MemberVO vo = new MemberVO(id,pwd,name,email);
		MemberService service = new MemberService();
		boolean flag = service.createMember(vo);
		if (flag) {
			out.print("<h1>입력에 성공했습니다.</h1>");
		} else {
			out.print("<h1>입력에 실패했습니다.</h1>");
		}
	}
	
}

```

