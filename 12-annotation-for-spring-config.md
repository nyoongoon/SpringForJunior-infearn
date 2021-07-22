# 12강 - 어노테이션을 이용한 스프링 설정 1
: XML파일을 java 파일로 변경하기


- 기존의 방법 -> xml파일로 스프링 컨테이너 설정
- 새로운 방법 -> java파일로 스프링 컨테이너 설정 (어노테이션 활용)

### @Bean으로 메소드로 스프링 컨테이너 설정

``` java
@Configuration
public class MemberConfig{//applicationContext.xml을 대체할 자바 클래스
	//<bean id="stduentDao" class = "member.dao.StudentDao"/>
	@Bean 
	public StudentDao studentDao(){
		//패스에 있는 클래스가 반환값
		//bean id가 메소드이름
		return new StudentDad();
	}

	/*
	<bean id="registerService" class="member.service.StudentRegisterService">
		<constructor-arg ref="studentDao"></constructor-arg>
	</bean>*/
	@Bean 
	public StudentRegisterService registerService(){
		return new StudentRegisterService(studentDao()) //dao메소드를 매개변수 안에서 호출!
	}

	/*
	<bean id="dataBaseConnectionInfoDev" class="~.DataBaseConntectionInfo">
		<property name = "jdbcUrl" value="~"/>
	</bean>	
	*/
	@Bean
	public DataBaseConntectionInfo dataBaseConnectionInfoDev(){
		DataBaseConnectionInfo info = new DataBaseConntectionInfo();
		info.setJdbcUrl("~");
		return info;
	}

}
```

- List, Map 타입이 있을 경우
```xml

<bean id="beanId" class="~.ClassPath">
	<property name = "developers">
		<list>
			<value>kim</value>
			<value>park</value>
			<value>yoon</value>
		</list>	
	</property>
	<property name = "administrators">
		<map>
			<entry>
				<key>
					<value>kim</value>
				</key>
				<value>kim@naver.com</value>
			</entry>
		</map>
	</property>
/<bean>
```

```java
@Bean
public ClassPath beanId(){
	ClassPath cp = new ClassPath();
 	//List 주입
	ArrayList<String> developers = new ArrayList<>();
	developers.add("kim");
	developers.add("park");
	developers.add("yoon");
	cp.setDevelopers(developers);

	//Map 주입
	Map<String, String> administrators = new HashMap<String, String>(); 
	administrators.put("kim", "kim@naver.com");
	cp.setAdministrators(administrators);
	
	return cp;
}
 
```

### @Configuration, @Bean으로 설정한 스프링 컨테이너에서 빈 객체 꺼내기
``` java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(ClassName.class);
ctx.getBean("~");
```
