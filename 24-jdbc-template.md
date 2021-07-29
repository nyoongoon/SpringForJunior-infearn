# 24강 - JDBC Template
1. JDBC의 단점을 보완한 JdbcTemplate
2. DataSource 클래스
  

- JDBC
: 드라이버 로딩 -> DB 연결 -> SQL 작성 및 전송 -> 자원해제

### JDBC Template
- jdbcTemplate(드라이버 로딩, DB연결, 자원해제) -> SQL 작성 및 전송

### DataSource 클래스
- 데이터베이스 연결과 관련된 정보를 가지고 있는 DataSource는 스프링 또는 c3p0에 제공하는 클래스를 이용할 수 있다.
- 스프링 : org.springframework.jdbc.datasource.DriverManagerDataSource
- c3p0 : com.mchange.v2.c3p0.DriverManagerDataSource

### pom.xml에 db 의존 설정

```xml
<repositories>
	<repository>
		<id>oracle</id>
		<name>ORACLE JDBC</name>
		<url>http://maven.jahia.org/maven2</url>
	</repository>
</repositories>

<dependencies>
	<!-- ... -->
	<!-- DB -->
	<dependency> <!-- DBMS -->
		<groupId>com.oracle</groupId>
		<artifactId>ojdbc6</artifactId>
		<version>12.1.0.2</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
		<version>4.1.6.RELEASE</version>
	</dependency>	
	<dependency>	<!-- 둘 중 하나만 해도 ok-->
		<groupId>com.mchange</groupId>
		<artifactId>c3p0</artifactId>
		<version>0.9.5</version>
	</dependency>
</dependencies>
```
 

### DAO에서
- 1. c3p0 사용
```java
@Repository
public class MemberDao implements IMemberDao {

	private String driver = "~";
	private String url = "~";
	private String userId = "~";
	private String userPw = "~";

	private DriverManagerDataSource dataSource; //DataSource 객체 선언

	private JdbcTemplate template; //JDBC Template 객체 선언

	public MemberDao(){
		// 생성자에서 DataSource 생성 및 설정.
		dataSource = new DriverManagerDataSource();
		dataSource.setDriverClass(driver);
		dataSource.setJdbcUrl(url);
		dataSource.setUserId(userId);
		dataSource.setPassword(userPw);

		template = new JdbcTemplate();
		template.setDateSource(dataSource);
	}

	// 메소드 .. 

```

- 2. springframework 사용
```java
@Repository
public class MemberDao implements IMemberDao {

	private String driver = "~";
	private String url = "~";
	private String userId = "~";
	private String userPw = "~";

	private DriverManagerDataSource dataSource; //DataSource 객체 선언

	private JdbcTemplate template; //JDBC Template 객체 선언

	public MemberDao(){
		// 생성자에서 DataSource 생성 및 설정. API가 조금씩 다름 주의!
		dataSource = new DriverManagerDataSource();
		dataSource.setDriverClassName(driver); //여기선 setDriverClassName()
		dataSource.setUrl(url);	//Url
		dataSource.setUsername(userId); //Username
		dataSource.setPassword(userPw);

		template = new JdbcTemplate();
		template.setDateSource(dataSource);
	}
	// 메소드 ..
```

- JdbcTemplate을 통한 sql메소드 사용 
- insert() 
```java
public int memberInsert(Member member){
	int result = 0;

	String sql = "INSERT INTO member (memId, memPw, memMail) values(?, ?, ?)";
	result = template.update(sql, member.getMemId(), member.getMemPw(), getMemMail());
	return result;
}
```
- insert() -> PreparedStatmentCreator() 사용 
```java
public int memberInsert(Member member){
	int result = 0;

	String sql = "INSERT INTO member (memId, memPw, memMail) values(?, ?, ?)";
	result = template.update(new PreparedStatementCreator(){
		@Override
		public PreparedStatement createPreparedStatement(Connection conn) throws SQLException{
			PreparedStatement pstmt = conn.PreparedStatement(sql);
			pstmt.setString(1, member.getMemId());
			pstmt.setString(2, member.getMemPw());
			pstmt.setString(3, member.getMemMail());
			return pstmt;
		}
	});
}
```
- insert() -> PreparedStatementSetter() 사용
```java
public int memberInsert(Member member){
	int result = 0;

	String sql = "INSERT INTO member (memId, memPw, memMail) values(?, ?, ?)";
	result = template.update(sql, new PreParedStatementSetter(){
		@Override
		public void setValues(PreparedStatment pstmt) throws SQLException{
			pstmt.setString(1, member.getMemId());
			pstmt.setString(2, member.getMemPw());
			pstmt.setString(3, member.getMemMail());
		}
	});
}
```
- select() -> PreparedStatementSetter() 사용
``` java
public Member memberSelect(Member member){
	List<Member> members = null;

	String sql = "SELECT * FROM member WHERE memId = ? AND memPw = ?";

	members = template.query(sql, new PreparedStatementSetter(){
		@Override
		public void setValues(PreparedStatement pstmt) thorws SQLException{
			pstmt.setString(1, member.getMemId());
			pstmt.setString(2, member.getMemPw());
		}
	}, new RowMapper<Member>(){
		@Override
		public Member mapRow(ResultSet rs, int rowNum) throws SQLException{
			Member mem = new Member();
			mem.setMemId(rs.getString("memId"));
			mem.setMemPw(rs.getString("memPw"));
			mem.setMemMail(rs.getString("memMail"));
			mem.setMemPurcNum(rs.getInt("memPurcNum"));
			return mem;
		}
	});

	if(members.isEmpty()) return null;

	return members.get(0);
}
```

- select() -> PreparedStatementCreator() 사용
``` java
public Member memberSelect(Member member){
	List<Member> members = null;

	String sql = "SELECT * FROM member WHERE memId = ? AND memPw = ?";

	members = template.query(sql, new PreparedStatementCreator(){
		@Override
		public PreparedStatement createPreparedStatement(Connection conn) throws SQLException{
			PreparedStatement pstmt = conn.preparedStatement(sql);
			pstmt.setString(1, member.getMemId());
			pstmt.setString(2, member.getMemPw());
			return pstmt;
		}
	}, new RowMapper<Member>(){
		@Override
		public Member mapRow(ResultSet rs, int rowNum) throws SQLException{
			Member mem = new Member();
			mem.setMemId(rs.getString("memId"));
			mem.setMemPw(rs.getString("memPw"));
			mem.setMemMail(rs.getString("memMail"));
			mem.setMemPurcNum(rs.getInt("memPurcNum"));
			return mem;
		}
	});

	if(members.isEmpty()) return null;

	return members.get(0);
}
```
- select() -> RowMapper 객체만 이용
``` java
public Member memberSelect(Member member){
	List<Member> members = null;

	String sql = "SELECT * FROM member WHERE memId = ? AND memPw = ?";

	members = template.query(sql, new RowMapper<Member>(){
		@Override
		public Member mapRow(ResultSet rs, int rowNum) throws SQLException{
			Member mem = new Member();
			mem.setMemId(rs.getString("memId"));
			mem.setMemPw(rs.getString("memPw"));
			mem.setMemMail(rs.getString("memMail"));
			mem.setMemPurcNum(rs.getInt("memPurcNum"));
			return mem;
		}
	}, member.getMemId(), member.getMemPw()); // 이곳에 나머지 인자 넣는다.

	if(members.isEmpty()) return null;

	return members.get(0);
}
```

- select()-> Object 배열로 값 넣어주기
``` java
public Member memberSelect(Member member){
	List<Member> members = null;

	String sql = "SELECT * FROM member WHERE memId = ? AND memPw = ?";

	members = template.query(sql,new Object[]{member.getMemId(), member.getMemPw()},//Object배열을 인자로
	 new RowMapper<Member>(){
		@Override
		public Member mapRow(ResultSet rs, int rowNum) throws SQLException{
			Member mem = new Member();
			mem.setMemId(rs.getString("memId"));
			mem.setMemPw(rs.getString("memPw"));
			mem.setMemMail(rs.getString("memMail"));
			mem.setMemPurcNum(rs.getInt("memPurcNum"));
			return mem;
		}
	});

	if(members.isEmpty()) return null;

	return members.get(0);
}
```














