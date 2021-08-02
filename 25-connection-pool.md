# 25강 - 커넥션 풀
1. c3p0 모듈의 ComboPooledDataSource
2. 스프링 설정파일을 이용한 DataSource 설정
- 클래스에서 직접 생성하는 방법
```java
private ComboPooledDataSource dataSource;
private JdbcTemplate template;

public MemberDao(){
	dataSource = new ComboPooledDataSource();
	//ComboPooledDataSource는 예외처리 필요!
	try{
		dataSource.setDriverClass(driver);
		dataSource.setJdbcUrl(url);
		dataSource.setUser(userId);
		dataSource.setPassword(userPw);		
	}catch(PropertyVetoException e){
		e.printStackTrace();
	}

	template = new JdbcTemplate();
	template.setDataSource(dataSource);
}
```
- 스프링 컨테이너에서 생성 후 가져다 쓰기. (xml)
```xml
<beans:bean id = "dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<beans:property name="driverClass" value="~"/>
	<beans:property name="jdbcUrl" value="~"/>
	<beans:property name="user" value="~"/>
	<beans:property name="password" value="~"/>
	<beans:property name="maxPoolSize" value="~"/>
	<beans:property name="checkoutTimeout" value="~"/>
	<beans:property name="maxIdleTime" value="~"/>
	<beans:property name="idleConnectionTestPeriod" value="~"/>
</beans:bean>
```
- @Configuration 사용하여 자바파일에서 스프링 빈 설정
```java
@Configuration 
public class DBConfig{
	@Bean	
	public ComboPooledDataSource dataSource() throws PropertyVetoException{
		ComboPooledDataSource dataSource = new ComboPooledDataSource();

		dataSource.setDriverClass("~");
		dataSource.setJdbcUrl("~");
		dataSource.setUser("~");
		dataSource.setPassword("~");
		dataSource.setMaxPoolSize(200);
		dataSource.setCheckoutTimeout(60000);
		dataSource.setMaxIdleTime(1800);
		dataSource.setIdleConnectionTestPeriod(600);
	}
}
```

- -> 컨테이너에서 빈 가져다 쓰기
```java
@Repository
public class MemberDao implements IMemberDao{

	private JdbcTemplate template;

	@Autowirde
	public MemberDao(ComboPooledDataSource dataSource){
		this.template = new JdbcTemplate(dataSource);
	}
	//...
}
```

