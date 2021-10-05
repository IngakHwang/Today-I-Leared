# CSV data => DB로 넣기(Java)

## Data Class

```java
public class ScoreData {
	// 문제를 풀기 위한 데이터 클래스
	private int sno;				//학번
	private String email;			//이메일
	private int kor;				//국어점수
	private int eng;				//영어점수
	private int math;				//수학점수
	private int sci;				//과학점수
	private int hist;				//역사점수
	private int total;				//총점
	private String mgrCode;			//담임코드
	private String accCode;			//성취도
	private String localCode;		//지역코드
	
	public ScoreData() {}
	public ScoreData(int sno, String email, int kor, int eng, int math, int sci, int hist, int total, String mgrCode, String accCode, String localCode) {
		
		this.sno = sno;
		this.email = email;
		this.kor = kor;
		this.eng = eng;
		this.math = math;
		this.sci = sci;
		this.hist = hist;
		this.total = total;
		this.mgrCode = mgrCode;
		this.accCode = accCode;
		this.localCode = localCode;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public int getSci() {
		return sci;
	}

	public void setSci(int sci) {
		this.sci = sci;
	}

	public int getHist() {
		return hist;
	}

	public void setHist(int hist) {
		this.hist = hist;
	}

	public String getMgrCode() {
		return mgrCode;
	}

	public void setMgrCode(String mgrCode) {
		this.mgrCode = mgrCode;
	}
	
	public int getSno() {
		return sno;
	}

	public void setSno(int sno) {
		this.sno = sno;
	}

	public int getKor() {
		return kor;
	}

	public void setKor(int kor) {
		this.kor = kor;
	}

	public int getEng() {
		return eng;
	}

	public void setEng(int eng) {
		this.eng = eng;
	}

	public int getMath() {
		return math;
	}

	public void setMath(int math) {
		this.math = math;
	}

	public int getTotal() {
		return total;
	}

	public void setTotal(int total) {
		this.total = total;
	}

	public String getAccCode() {
		return accCode;
	}

	public void setAccCode(String accCode) {
		this.accCode = accCode;
	}

	public String getLocalCode() {
		return localCode;
	}

	public void setLocalCode(String localCode) {
		this.localCode = localCode;
	}
	
}

```



## Logic Class

```java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;

public class DatabaseTest {

	public static void main(String[] args) {
		DatabaseTest dt = new DatabaseTest();					

		try {
			dt.insertDataFromFile();
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}		
	}
    
    private ArrayList<ScoreData> makeList(){
		//파일에 접속해서 한라인 씩 읽어서 인스턴스 만드는 작업
		ArrayList<ScoreData> list = new ArrayList<ScoreData>();
		
		//list 구성
		File file = new File("./data/Abc1115.csv");				//파일주소
		FileReader fr = null;
		BufferedReader br = null;
		try {
			fr = new FileReader(file);
			br = new BufferedReader(fr);
			String line = null;
			while((line=br.readLine())!=null){					//파일을 한 줄씩 끝까지 읽기
				//System.out.println(line);
				String[] info = line.split(",");				//String ,구분 배열삽입 
				int sno = Integer.parseInt((info[0]));
				String email = info[1];
				int kor = Integer.parseInt((info[2].trim()));
				int eng = Integer.parseInt((info[3].trim()));
				int math = Integer.parseInt((info[4].trim()));
				int sci = Integer.parseInt((info[5].trim()));
				int hist = Integer.parseInt((info[6].trim()));
				int total = Integer.parseInt((info[7].trim()));
				String mgr = info[8];
				String acc = info[9];
				String local = info[10];
				
				list.add(new ScoreData(sno,email,kor,eng,math,sci,hist,total,mgr,acc,local));	//객체 넣기
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		try {
			br.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		try {
			fr.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return list;
	}
    
    public Connection makeConnection() throws ClassNotFoundException, SQLException {
        //Java와 DB 연결을 위한 메소드
        
		Connection con = null;								//driver로딩담기 = Connection
		
		//DB연결을 위해서 4가지 정보가 필요
		String driver = "oracle.jdbc.OracleDriver";			//패키지
		String url = "jdbc:oracle:thin:@localhost:1521:xe";	//주소(프로토콜 : @주소)
		String id = "HR";									//DB 로그인 아이디
		String pwd = "1234";								//DB pw
		
		Class.forName(driver);								//클래스.class

		con = DriverManager.getConnection(url,id,pwd);		//DB와 Java 연결 (Driver로드)
		//연결에 성공하면 DB 와 연결 상태를 Connection 객체로 표현하여 반환
        //Connection은 네트워크상의 연결자체를 의미 (Java와 DB사이의 길)
        //Connection 하나당 트랜잭션 하나를 관리 
        //(트랜잭션은 하나 이상의 쿼리에서 동일한 Connection 객체를 공유하는 것)
		return con;
	}

	public void insertDataFromFile() throws SQLException, ClassNotFoundException {
        //문제 : csv파일에 있는 data -> DB로 옮기기 (1000줄)
		//1. 파일에서 한라인 읽어서 DB에 삽입,반복 --> 좋지 않은 방법
		
		//2. 파일에서 모든 라인을 읽어서 정리하고 한번에 DB에 넣기 --> 좋은 방법
        //how?
		//2-1. 하나의 라인을 어떤 방법으로 DB에 삽입할 것인가 --> 인스턴스로 만들어서 처리하는 방법
		//1000개의 데이터 인스턴스를 만들어서 처리한다. 
        
		ArrayList<ScoreData> list = this.makeList();		//위 메소드 실행 후 list에 초기화
        
		String sql = "INSERT INTO studentTBL VALUES"+								
				"(?,?,?,?,?,?,?,?,?,?,?)";  				//쿼리문 placeholder => 들어가야할 data 영역표시
		Connection con = this.makeConnection();				//DB와 java 연결
		PreparedStatement stmt = con.prepareStatement(sql);	//prepare
		//3. DB에 삽입을 어떻게 할것인가?
		for(int i=0; i<list.size() ; i++){				
			ScoreData temp = list.get(i);				//데이터 인덱스 하나씩
			
			//placeholder(?) 자리에 알맞은 데이터 셋팅하는 작업 --> 11개 작업을 해야함
			stmt.setInt(1, temp.getSno());				//Number, primaryKey
			stmt.setString(2, temp.getEmail());			//Char
			stmt.setInt(3, temp.getKor());				//Number
			stmt.setInt(4, temp.getEng());				//Number
			stmt.setInt(5, temp.getMath());				//Number
			stmt.setInt(6, temp.getSci());				//Number
			stmt.setInt(7, temp.getHist());				//Number
			stmt.setInt(8, temp.getTotal());			//Number
			stmt.setString(9, temp.getMgrCode());		//Char
			stmt.setString(10, temp.getAccCode());		//Char
			stmt.setString(11, temp.getLocalCode());	//Char
			
			
			int affectedCount =stmt.executeUpdate();	//stmt가 execute되면 숫자반환		
			if (affectedCount>0) {														
				System.out.println("삽입 작업이 완료되었습니다.");
			}
		}
		
		stmt.close();
		con.close();
	}
	
	
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
	
	public void deleteData() throws ClassNotFoundException, SQLException {
		//stdno가 990001인 데이터를 삭제하시오.
		String sql = "DELETE FROM studentTBL WHERE stdno = 990001";
		Connection con = this.makeConnection();
		Statement stmt = con.createStatement();
		int affectedCount =stmt.executeUpdate(sql);									
		if (affectedCount>0) {														
			System.out.println("삭제 작업이 완료되었습니다.");
		}
		stmt.close();
		con.close();
		
	}

}
```



