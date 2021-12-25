# Java - DB_CRUD

> Java CRUD 요약

 

## INSERT

```java
//PreparedStatement
public void insertData(String name, String addr) throws SQLException{
		String sql = "INSERT INTO stdTBL VALUES(?,?)";
		Connection con = this.makeconntion();
		//stdTBL 접속
		PreparedStatement st = con.prepareStatement(sql);
		st.setString(1, name);
		st.setString(2, addr);
		int result = st.executeUpdate();
		
		st.close();
		con.close();
		
	}

//Statement
public void insertData() throws ClassNotFoundException, SQLException {
		// DB 내 StudentTBL table에 데이터를 삽입
		String sql = "INSERT INTO studentTBL VALUES"+							//변수 선언
						"(990001,'addx', 17, 29, 16, 49, 43,154,'C','A','C')";
		Connection con = this.makeConnection();													//connection 객체에 위 메소드(DB연결) 연결
		Statement stmt = con.createStatement();													//statement 객체 생성 (con 객체의 메소드 이용)
		int affectedCount =stmt.executeUpdate(sql);							  					//executeUpdate = table에 영향을 준다 => CREATE, UPDATE, DELETE 중 하나
		if (affectedCount>0) {
			System.out.println("삽입 작업이 완료되었습니다.");
		}
		stmt.close();
		con.close();
	}
```

## UPDATE

```java
public void updateData() throws ClassNotFoundException, SQLException {
		//addx인 것을 mult로 수정하시오.
		String sql = "UPDATE studentTBL SET email='mult'WHERE stdno=990001";
		Connection con = this.makeConnection();
		Statement stmt = con.createStatement();				//쿼리를 보내는 통로
		int affectedCount =stmt.executeUpdate(sql);			//업데이트된 갯수를 리턴해줌					
		if (affectedCount>0) {														
			System.out.println("수정 작업이 완료되었습니다.");
		}
		stmt.close();
		con.close();
		
	}
```

## DELETE

```java
public void deleteData() throws SQLException {
		String sql = "DELETE FROM stdTBL WHERE STDNAME = '유나얼'";
		Connection con = this.makeconntion();
		//stdTBL 접속
		Statement st = con.createStatement();
		int result = st.executeUpdate(sql);
		if (result>0) {
			System.out.println("삭제완료");
		}

		
		st.close();
		con.close();
	}
```

## SELECT

```java
public void readAll() throws SQLException{
		String sql = "SELECT * FROM stdTBL";
		Connection con = this.makeconntion();
		//stdTBL 접속
		Statement st = con.createStatement();
		ResultSet rs = st.executeQuery(sql);
		
		while(rs.next()) {
			String stdName = rs.getString(1);
			String addr = rs.getString(2);
			
			System.out.println(stdName+","+addr);
		}
		rs.close();
		st.close();
		con.close();
		
	}
```

