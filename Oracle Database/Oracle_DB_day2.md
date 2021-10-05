# Oracle Datebase day2

## SQL CRUD 중 C,U,D

### CREATE

> DB or TABLE 생성을 위한 명령어

**`CREATE {DATEBASE or TABLE} {이름};` => DB or table 생성**

```sql
CREATE DATABASE exDB;


--table은 필드명, 데이터 유형기재해서 선언해야 함
CREATE TABLE exTable(
	id NUMBER(4),
	userName NCHAR(3),
	age NUMBER(2)
);
```



### INSERT

> TABLE 내 data 입력

**`INSERT INTO TABLE[(열1,열,2...)]VALUES(값1,값2...)` =>table 내 데이터 입력**

- TABLE 옆 []내 열 생략 가능

```sql
--table(열) 생략 가능
--생략 시 , VALUES 값 순서 및 개수를 TABLE에 정의된 순서와 개수 동일하게 입력

INSERT INTO testTBL1 VALUES(1,'홍길동',25);

INSERT INTO testTBL1(id, userName) VALUES(2,'설현');

--열의 순서를 바꿔서 입력가능
INSERT INTO testTBL1(userName, age, id) VALUES('지민',26,3);
```



### UPDATE

> TABLE 내 data 수정

**`UPDATE TABLE SET 열1 = 값1, 열2 = 값2 WHERE 열 = 값` => table 내 데이터 수정**

- WHERE 생략 가능하나 생략 시 테이블 전체 행 수정

```sql
UPDATE testTBL1
	SET Address = '경기',
	PW = '4567',
	WHERE = 'user1';
	
--WHERE 생략
UPDATE buyTBL
	SET Price = price * 15;
```



### DELETE 

> TABLE 내 data 삭제

**`DELETE FROM TABLE WHERE '조건'` => table 내 데이터 삭제**

- 행 단위로 삭제

```sql
DELETE FROM testTBL1 WHERE ID = 'osw9123';
```



## Java => Oracle DB 연결

### JDBC(Java DataBase Connectivity) 

> 자바와 다양한 RDBMS에 접속하고 SQL문 수행하고자 할때 사용되는 API

#### 실행과정

1. **JDBC Driver Load**	(Java에서 DB 접속을 위한 Driver로딩)
   - Class.forName("*Driver패키지명기재*")
2. **Connection 객체 만들기** (DB연결), (DB와 Java간의 통로 연결)
   - Connection con = DriverManager.getConnection("DB주소", "id","pwd")
3. **Statement 객체 만들기**(SQL문 실행객체)
   - Statement st = con.createStatement()
   - PreparedStatement ppst = con.prepareStatement("*sql*") => 쿼리 작성 시placeholder 기입
4. **SQL 문 전송**
   - st.executeUpdate("sql");
   - ppst.executeUpdate(); => PreparedStatement 선언시 쿼리 작성했음
5. **결과 받기** (Query 결과 변수에 저장)
   - int result = st.executeUpdate("sql");
6. **연결 해제** (Statement, Connection 객체 연결 종료)
   - st.close() || ppst.close()
   - con.close();





