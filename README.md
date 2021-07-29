# SpringForJunior-inflearn
자바 스프링 프레임워크(renew ver.) - 신입 프로그래머를 위한 강좌를 듣고 기록하는 저장소입니다. 
<br>
<br>

# 1강 - 스프링 개요 

- DI 
- AOP : 관점 지향 프로그래밍 
- MVC. 
- JDBC. 

## 프레임워크
 : 개발자들의 업무를 추상적으로 정의해 놓은 틀.

## 스프링 프레임워크 모듈

		스프링 모듈 
		spring-core      : 	 스프링 핵심 기능인 DI와 IoC 기능 
		spring-aop       :   AOP구현 기능 
		spring-jdbc       :  DB 쉽게 다룰 수 있는 기능
		spring-tx 	       :  트랜잭션 관련 기능
		spring-webmvc : 컨트롤러와 뷰를 이용한 스프링 MVC 구현 기능

- 스프링 프레임워크에서 제공하고 있는 모듈을 사용하려면, 모듈에 대한 의존설정을 개발프로젝트에 XML 파일 들을 이용해 서 개발자가 직접 하면 된다. 

## 스프링 컨테이너. (IoC)
-  스프링에서 객체를 생성하고 조립하는 컨테이너(container)로, 컨테이너를 통해 생성된 객체를 빈(Bean)이라고 부른다.

<br>
<br>

# 3 강 - 스프링 프로젝트 시작하기
## New Maven Project

- Group id : 가장 넓은 프로젝트 이름
- Artifact id : 현재 프로젝트의 이름

## pom.xml 작성
- 필요한 모듈을 가져오기 위한 파일. 
- \<dependencies\> 
- \<build\> 
 

## 스프링 프로젝트 구조 
- src/main/java : 자바 파일 관리
- src/main/resources : 자원 파일 관리. 스프링 설정파일(xml) 또는 프로퍼티 파일 등이 관리됨.

## 폴더 및 pom.xml 파일의 이해
- pom.xml 파일은 메이븐 설정 파일로 메이븐은 라이브러리를 연결해주고, 빌드를 위한 플랫폼
- pom.xml에 의해서 필요한 라이브러리를 레퍼지토리에서 다운로드해서 사용.
- cf) 모듈의 라이브러리 파일명은 artifactId + “-“ + 버전명 + “.jar”로 표시됨.

<br>
<br>

# 4강 - 스프링 컨테이너에 접근하기 위해 xml 파일 생성하기
## 스프링 프로젝트 시작 -> 스프링 컨테이너에 객체 로드하기 위해  xml파일 생성하기.
- resources에 applicationContext.xml 생성
- \<beans\> \<bean id =“tWalk” class=“testPjt.TranspotationWalk”/\> \</beans\>
- -> new로 만들지 않고. bean태그에 의해서 자동으로 객체 생성 (메모리에 객체 로드) 
- -> 스프링에서 객체들이 로드된 메모리를 스프링 컨테이너라고 부름. 

## 스프링 컨테이너에 접근하는 방법
- 메모리에 로드된 객체를 꺼내오기 위해 컨테이너에 접근해야 함.
- GenericXmlApplicationContext -> 컨테이너에 접근하기 위한 데이터 타입

``` Java
GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(“classpath:applicationContext.xml”);
//빈 가져오기							//빈 id , 클래스 위치 
TranspotationWalk transpotationWalk =  ctx.getBean(“tWalk, TranspotationWalk.class”);
// 사용후 반환
ctx.close();
```
<br>
<br>
 
# 5강 - 다른 방식으로 (로컬에서) 프로젝트 생성하기
## 또 다른 프로젝트 생성 방법
1 로컬에서 만들기.
2 이클립스에서 import하기.

## 1. 로컬에서 프로젝트 폴더와 pom.xml파일 만들기

- 프로젝트 폴더  - src - main - java
	     \		 \ 
	        pom.xml      resources

 - pom.xml 파일 작성할 때, 프로젝트 id 주의 

 - import - maven - project 
<br>
<br>

# 6강 - 의존성 주입(생성자)
- 스프링 컨테이너의 기본 개념
- DI 기본 개념


## DI란?
- ex) 배터리 일체형 장난감 // 배터리 분리형 장난감
- 배터리가 떨어지면 새로 구입 // 배터리만 교체
- -> 프로그램을 쉽게 유지보수 

``` java
public class ElectronicCarToy{
	private Battey battery;
	public ElectronicCarToy(){
		battery = new NormalBatter();
	}	// 배터리 일체형
}
``` 
- 내부에서 객체 생성.

``` java
public class ElectronicRobotToy{
	private Battery battery;
	public ElectronicRobotToy(){
		// ㄴ> 내부에서 객체 생성하지 않음
		// 생성자에서 객체를 받아오기도 함.
		// (수정할떄만 외부에서 받아오는 경우)
	}
	public void setBattery(Battery battery){
		this.battery = battery;
		// 외부에서 객체 주입 받음
		// Battery는 인터페이스로 만들어서 다형성 확보
	}
}
```
- 외부에서 객체를 주입 받아옴.

## 스프링 DI 설정 방법

								
		스프링 설정파일로 스프링 컨테이너 생성   			    ---->				 스프링 컨테이너에서 빈 객체 꺼내기
		(applicationContext.xml)		GenericXmlApplicaionContext(				getBean(id, classpath);
								"classpath:applicationContext.xml");

- applicationContext.xml에서 **생성자로 의존성 주입하기
		
``` xml		
<beans>
	<bean id = "studentDao" class = "~~"></bean>
	<bean id = "registerService" class = "~~">
		<constructor-arg ref="studentDao"></constructor-arg>    <!-- 여기서 생성자로 의존성 주입을 하고 있음!-->
	</bean>
</beans>
```
<br>
<br>

# 7강 - 다양한 의존 객체 주입 방법

### 1. 생성자를 이용한 의존 객체 주입
``` java
public StudentRegisterService(StudentDao, studentDao){ //생성자
	this.studentDao = studentDao;
}
```
- applicationContext.xml에서 constructor-arg 엘리먼트 사용. 

``` xml
<bean id="registerService" class="~~">
	<constructor-arg ref="studentDao></constructor-arg>"
</bean>	
```

### 2. setter 메소드를 이용한 의존 객체 주입
```java
//셋터 메소드
public void setJdbcUrl(String jdbcUrl){
	this.jdbcUrl = jdbcUrl
}

```
- 셋터 메소드에서 set생략이 된 부분이 property 엘리먼트 name
``` xml
<bean id="dataBaseConnectionInfoDev" class="~~">
	<!--셋터 메소드에서 set생략이 된 부분이 property의 이름이 됨-->
	<property name = "jdbcUrl" value="~"/>
</bean>	
```


### 3. List타입 의존 객체 주입
```java
public void setDevelopers(List<String>developers){
	this.developers = developers;
}
```
- 세터 메소드의 매개변수가 List인 경우 list 엘리먼트 사용
```xml
<property name="developers">
	<list> <!--매개변수로 들어가는 List 설정 -->
		<value>kim</value>
		<value>park</value>
		<value>yoon</value>
	</list>
</property>
```

### 4. Map타입 객체 주입
```java
public void setAdministrators(Map<String, String> administrators){
	this.administrators = administrators;
}
```

```xml
<property name = "administrators">
	<map>
		<entry> <!-- 엔트리안에 키, 값-->
			<key> <!-- 키 -->
				<value>kim</value>
			</key><!-- 값 -->
			<value>kim@naver.com</value>
		</entry>
		<entry> <!-- 엔트리안에 키, 값-->
			<key> <!-- 키 -->
				<value>park</value>
			</key><!-- 값 -->
			<value>park@naver.com</value>
		</entry>
	</map>
</property>
```
<br><br>

# 8강 - 스프링 설정 파일 분리 // 빈의 범위
### 스프링 설정파일 분리하기 
- 스프링 설정 파일(applicationContext.xml)에서 많은 설정해두기 때문에 파일이 너무 길어질 수가 있다.  
- -> 스프링 설정 파일 분리
- /src/main/resources에 xml파일을 기능에 따라 분리해서 보관
ex) appCtx1.xml, appCtx2.xml, appCtx3.xml, 로 분리 한 경우.
``` java
String[] appCtxs = {"classpath:appCtx1.xml", "classpath:appCtx2.xml", "classpath:appCtx3.xml"};
GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(appCtxs);
// 스트링 배열로 xml파일 path전달
// 주로 사용하는 방식
```

- 또는 xml파일 내에서 import엘리먼트를 사용해서 분리된 설정 파일을 합칠 수도 있다. 
<br/>

### 빈의 범위
- 싱글톤 // 프로토타입

: 스프링 컨테이너에서 생성된 빈 객체의 경우 동일한 타입에 대해서는 기본적으로 한개만 생성이 되며, getBean() 메소드로 호출될 때 동일한 객체가 반환된다. 
- 싱글톤
``` java
Object a = ctx.getBean("A");  
Object b = ctx.getBean("A");
// a와 b는 동일한 객체를 참조한다.
```
cf) new ClassName(); --> 각각 다른 객체

- 프로토타입
: 싱글톤과 반대되는 개념으로 스프링 설정파일에서 Bean객체를 정의할 때 scope속성을 명시해주면 된다.
```xml
<bean id = "~" class = "~" scope="prototype">
</bean>
```
<br><br>

# 9강 - 의존객체 자동 주입

### 의존객체 자동 주입이란?
- 의존 객체 자동 주입
: 스프링 설정 파일에서 의존객체를 주입할 때, 태그를 통해 의존 대상 객체를 명시하지 않아도 스프링 컨테이너가 자동으로 필요한 의존 대상 객체를 찾아서 의존 대상 객체라 필요한 객체에 주입해주는 기능이다. 
: @Autowired와 @Resource 어노테이션을 이용해서 쉽게 구현 가능.


### @Autowired
- 주입하려고 하는 객체의 **타입**이 일치하는 객체를 자동으로 주입한다. 
``` xml
<beans<!-- 네임스페이스, 스키마 설정을 추가-->>

	<context:annotation-config/>
	<!-- 
		기존 객체 주입 설정 삭제
	-->
</beans>

```
- 객체 주입이 필요한 코드 위(생성자, 프로퍼티, 메소드)에 @Autowired 작성.
- 프로퍼티나 메소드에 @Autowired 사용할 때, 반드시 디폴트 생성자를 명시해 주어야함!

### @Resource
- 주입하려고 하는 객체의 **이름**이 일치하는 객체를 자동으로 주입한다. 
- 생성자에는 사용 불가능. 프로퍼티 또는 메소드만!
- 디폴프 생성자 명시해줘야함. 

<br/>
<br/>

# 10강 - 의존 객체 선택
1. 의존 객체 선택
2. 의존 객체 자동 주입 체크
3. @Inject
<br/>
- 의존 객체가 자동으로 주입될 때 스프링 컨테이너에서 어떻게 의존 객체가 선택되는지 .
- 다수의 빈 객체 중 의존 객체의 대상이 되는 객체를 선택하는 방법. 
<br/>

### 의존 객체 선택
- 동일한 객체가 2개 이상인 경우 스프링 컨테이너는 자동 주입 대상 객체를 판단하지 못해서 예외를 발생시킨다. 
- -> qualifier 태그 사용
- xml파일에서
```xml
<bean id ="~" class="~">
	<qualifier value ="usedDao"/>
</bean>
```
- java파일에서
```java
@Autowired
@Qualifier("usedDao");
private WordDao wordDao;
```

### 의존 객체 자동 주입 체크 (잘 안씀)
- 스프링 설정파일에 주입해야할 빈 파일이 설정 안 된 경우.
```java
 @Autowired(required = false)
```
- -> 없으면 예외 발생시키지 말고 그냥 주입하지 마라.

### @Inject
- @Autowired와 거의 비슷하게 @Inject 어노테이션을 이용해서 의존 객체를 자동으로 주입할 수 있다.
- @Inject은 required 속성을 지원하지 않는다. 
cf) @Autowired가 더 많이 쓰임
- @Named : @Qualifier와 비슷한 기능
```xml
<bean id="wordDao1" class = "~.WordDao"/>
```
``` java
@Inject
@Named(value="wordDao1")
private WordDao wordDao
```

<br/>
<br/>

# 11강 - 생명주기 (인터페이스orXML이용해서 빈 생성, 소멸 메소드 작성하기)
1. 스프링 컨테이너 생명주기
2. 빈 객체 생성주기
3. init-method, destroy-method 속성

### 스프링 컨테이너 생명주기
- GenerixXmlApplicaionContext를 이용한 스프링 컨테이너 초기화(컨테이너 생성) (+ 컨테이너 생성과 동시에 빈 객체들도 생성됨)
- getBean()을 이용한 빈 객체 꺼내쓰기(사용)
- close()를 이용한 스프링 컨테이너 종료(컨테이너 소멸)

### 빈 객체 생명주기 
- 스프링 컨테이너 초기화 -> 빈객체 생성 및 주입
- 스프링 컨테이너 종료 -> 빈 객체 소멸
- -> 빈 객체의 생명주기는 스프링 컨테이너 생명주기와 같다. 

			<interface>   			<interface>
			InitializingBean		DisposableBean
			afterPropertiesSet()		destroy()
				 ↑			    ↑
					<빈(Bean) 객체>
					afterPropertiesSet() <- 빈객체 생성 시점에 호출
					destroy()	<- 빈객체 소멸 시점에 호출

- 자바 파일에서 -> InitializingBean // DisposableBean 인터페이스 구현
- 빈 객체가 생성 될 때 특정 작업을 작성 -> afterPropertiesSet()
- 빈 객체가 소멸 될 때 특정 작업을 작성 -> destroy()

### init-method, destroy-method 속성
: 빈 객체가 생성\소멸 될 때 특정 작업 구현을 xml에서 설정할 수 있음!
- xml 파일에서 설정하기
``` xml
<bean id = "~" class = "~" init-method = "initMethod" destroy-method = "destroyMethod"/>
```
- java파일에서 같은 이름의 메소드 작성
``` java
public void initMethod(){//이름 일치
	//내용 작성
}
public void destroyMethod(){//이름 일치
	//내용 작성
}
```
<br/>
<br/>

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

<br/>
<br/>

# 13 강 - 어노테이션을 이용한 스프링 설정 2(saparate config)
1. Java 파일 분리
2. @Import 어노테이션

- xml파일을 분리하여 스프링 설정하는 것처럼 java파일을 분리하여 스프링 설정을 할 수 있다.
- 일반적으로 기능별로 분리. 
cf) 이클립스 ctl + shift + o -> import추가 / 정리

### 설정 분리하면서도 참조해야할 부분이 있을 때
- 설정 파일에 @Autowired를 설정하여 프로퍼티 생성.

``` java
@Configuration
public class MemberConfig1{
	@Bean
	public DataBaseConnectionInfo dataBaseConnectionInfoDev(){
		DataBaseConnectionInfo infoDev = new DataBaseConnectionInfo();
		// ...
		return infoDev;
	}
}
```
- 위 아래로 페이지가 분리되었지만 참조해야할 경우
``` java
@Configuration
public class MemberConfig2{
	//autowired로 자동 주입. -> 필요한 객체가 자동을 주입 됨. -> 메소드를 사용할 수 있음. 
	@Autowired
	DataBaseConnectionInfo dataBaseConnectionInfoDev;
	@Bean
	public Returntype method(){
		//...
		//다른 페이지에 있는 객체의 메소드가 필요한 경우
		args.put("dev", dataBaseConnectionInfoDev());
	}
 //...

}
```

### 분리된 설정파일로 컨테이너 생성
```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(MemberConfig1.class, MemberConfig2.class);
// 매개변수로 각각 넣어준다. 
```

### @Import 어노테이션
- 매개변수로 각각 넣지 않고 하나로 합치기 위해 @Import 사용
``` java
@Configuration
@Import({MemberConfig2.class, MemberConfig3})
public class MemberConfig1{
	//..
}
```
```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(MemberConfig1.class);
// 합쳐진 하나의 설정 클래스 파일만 넣는다.
```

<br/>
<br/>

# 13강 - 웹 프로그래밍 설계 모델
1. 웹 프로그래밍을 구축하기 위한 설계 모델
2. 스프링 MVC 설계 구조
3. DispatcherServlet 설정
4. Controller 객체 - @Controller
5. Controller 객체 - @RequestMapping
6. Controller 객체 - Model 타입의 파라미터
7. View 객체
8. 전체적인 웹 프로그래밍 구조

### MVC 기본 구조

		 	   -> Controller <-> Service <-> DAO <-> Model <-> DB
		브라우저		  ↓
			   <-    View

### 스프링 MVC 설계 구조
- 스프링 설정파일로 스프링 컨테이너가 만들어지면 HandlerMapping, HandlerAdapter, ViewResover는 컨테이너 안에 자동 생성 됨!
 
		    	1.HandlerMapping    2.HandlerAdapter <-> Controller (<-> Service <-> DAO <-> Model <-> DB)
				 /           /        (요청처리)
		브라우저 ----->  DispathcerServlet 
			↑(응답)	 \(응답생성)   \(처리결과를 출력할 view선택)
			------- 4.View	   3.ViewResolver


1. 브라우저 -(HttpRequest)-> DispatcherServlet -> HandlerMapping : 알맞은 컨트롤러 선택 -> DispathcerServlet
2. DispatcherServlet -> HandlerAdapter : 선택된 컨트롤러에서 적합한 메소드를 찾아서 컨트롤러&메소드 호출 -> model&view를 받아서 DispatcherServlet반환 
3. DispatcherSerlvet -> ViewResolver : 받아온 model&view을 사용할 jsp문서 선택 -> DispatcherSerlvet 
4. DispatcherSerlvet -> View(jsp) -(HttpResponse)-> 브라우저 

### DispatcherSerlvet 설정 (프레임워크가 생성)
- web.xml에 서블릿을 매핑
``` xml
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
	</init-param>					<!-- ㄴ> 스프링 설정파일 -->		
	<load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
	<servlet-name>appServlet</servlet-name>
	<url-pattern>/</url-pattern> <!-- 클라이언트의 모든 요청을 DispatcherServlet에게 보냄-->
</servlet-mapping>
``` 
- init param을 설정했을 경우
: serlvet-context.xml을 이용하여 스프링 컨테이너가 생성 됨

- init param을 설정하지 않았을 경우
:  스프링 프레임워크가 appServlet-context.xml(미리 작성해둔 설정파일)을 찾아와서 컨테이너 생성


### Controller 객체 - @Controller
- 스프링 프레임워크에서 백엔드 개발자가 주로 작업하는 것. (controller-service-dao-db)
- Controller에서는 알맞은 model객체와 view객체를 담아서 DispatcherServlet 에 전달해주는 역할을 한다.

- xml파일
``` xml
<!-- servlet-context.xml -->
<annotation-driven/>
```
- java파일
``` java
@Controller
public Class HomeConroller{
	//..
}
```

- HandlerAdapter가 요구한 메소드 매핑 설정
```java
@RequestMapping("/success")
public String success(Model model){
	return "success";
}
```

- DispatcherServlet에게 전달해줄 모델 객체 설정 

				    @ReuqestMapping("/success")
				    public String success(Model model){
							     ↓
			뷰에서 사용 ->	model.setAttribute("속성이름", "속성값")	    


### ViewResolver (프레임워크가 생성) 
- 스프링 설정파일에서
``` xml
<beans:bean class="org.springframeword.web.servlet.view.InternalResourceViewResolver">
	<beans:property name="prefix" value="/WEB-INF/views/"/>
	<beans:property name="suffix" value=".jsp"/>
</beans:bean>
```	

		<beans:property name="suffix" value=".jsp"/>								
							↓	
		 -> JSP파일경로 : /WEB-INF/views/success.jsp
						↑(컨트롤러에서)
			public String success(Model model){
				return "success";
			}


<br/>
<br/>

# 15강 - 스프링 MVC 웹서비스 
1. 프로젝트 전체 구조
2. web.xml
3. DispatcherServlet
4. servlet-context.xml
5. Controller
6. View

### 프로젝트 전체 구조
- java 폴더  : 웹애플리케이션에서 사용되는 Controller, Service, DAO 객체들이 위치
- resources 폴더 : JSP파일을 제외한 html, css, js파일들이 위치
- webapp 폴더 : 웹과 관련된 파일들(스프링 설정파일, JSP파일, HTML파일, web.xml파일 등)이 위치 
- spring 폴더 : 스프링 컨테이너를 생성하기 위한 스프링 설정파일이 위치 .
- views 폴더 : View로 사용될 JSP파일이 위치
- pom.xml파일 : 메인 레파지토리에서 프로젝트에 필요한 라이브러리를 내려받기 위한 메이븐 설정 파일 

### web.xml (웹 설정 -> 프론트 컨트롤러 등록)
: 웹어플리케이션에서 **최초 사용자 요청**이 발생되면 가장먼저 DispatcherServlet이 사용자의 요청을 받기 때문에 개발자는 DispatcherServlet을 서블릿으로 등록해주는 과정을 설정해주어야한다. 그리고 사용자의 모든 요청을 받기 위해서 서블릿 맵핑 경로는 /로 설정한다. 	

``` xml
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class> 
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		<!--서블릿 초기화 매개변수 : 서블릿을 생성하고 초기화 할 때, 즉 서블릿 컨테이너가 'init()'을 호출할 때 전달하는 데이터. 보통 데이터베이스에서 연결 정보, 시스템 환경 정보 같은 정적인 데이터를 서블릿에 전달할 때 사용 -->
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
	<servlet-name>appServlet</servlet-name>
	<url-pattern>/</url-pattern> <!-- 사용자의 모든 요청을 받기 때문에 /로 설정 -->
</servlet-mapping>

```

### DispatcherServlet (프론트 컨트롤러 -> 요청을 받아 페이지 컨트롤러를 찾게 도와줌)
1. 사용자의 모든 요청을 DispatcherServlet이 받은 후 HandlerMapping객체에 **Controller 객체 검색** 요청을 한다. <br/> 그러면 HandlerMapping 객체는 프로젝트에 존재하는 모든 Controller 객체를 검색하여 적합한 객체를 찾은 후, DispatcherServlet 객체에게 알려준다.
2. DispatcherServlet 객체는 HandlerAdapter객체에 사용자 요청에 부합하는 **메소드 검색**을 요청한다. <br/> 그러면 HandlerAdapter 객체는 사용자의 요청에 부합하는 메소드를 찾아 해당 Controller객체의 메소드를 실행한다. 
3. 선택된 Controller 객체의 메소드가 실행된 후, Controller 객체는 사용자 응답에 필요한 데이터정보와 뷰정보가 담긴 ModelAndView객체를 HandlerAdapter 객체에 반환한다.
4. 마지막으로 HandlerAdapter객체는 받은 ModelAndView 객체를 DispatcherSerlvet 객체에 반환한다.


### serlvet-context.xml (스프링 설정 파일 -> 프론트 컨트롤러의 초기화 매개변수로 삽입)
: web.xml에서 DispatcherSerlvet을 등록할 때, contextConfigLocation 이름으로 초기화 파라미터 servlet-context.xml을 지정했는데, 이 파일이 바로 **스프링 설정**의 역할을 하는 파일이다.
: 스프링 설정파일은 프로젝트에 필요한 객체(자바빈)을 담아 스프링 컨테이너를 생성한다. 

```xml
<!-- ... -->

	<!-- 리소스 폴더 매핑(프론트 관련 파일 위치) -->
	<resources mapping="/resources/**" location="/resources/" />
	
	<!-- prefix와 suffix로 jsp파일의 위치를 찾아주는 ViewResolver 빈 -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>

<!-- ... -->
```

### Controller(페이지 컨트롤러)
: Controller (-> Service -> DAO)
: 사용자의 요청을 실제로 처리하는 객체. 개발자가 주로 작업해야하는 파일.

``` java
@Controller //컨트롤러임을 나타내는 어노테이션. (HandlerMapping빈이 모든 컨트롤러를 뒤져 적합한 컨트롤러를 찾아낼 때 필요)
public class HomeController {
	
	//value == 사용자로부터 "/"요청이 들어오면 실행되는 메소드
	@RequestMapping(value = "/", method = RequestMethod.GET) //HandlerAdapter가 찾을 수 있게 도와주는 어노테이션
	public String home(Locale locale, Model model) {
				        	// ㄴ> view에서 사용될 데이터를 담은 모델 객체
		//사용자의 요청을 실제로 처리하는 로직을 작석

		return "home"; // 알맞은 jsp파일 위치를 ViewResoiver에 String으로 리턴한다.
	}
}
```


<br/>
<br/>

# 16강 - STS를 이용하지 않은 웹프로젝트
1. 스프링 MVC 웹 애플리케이션 제작을 위한 폴더 생성
2. pom.xml 및 이클립스 import
3. web.xml 작성
4. 스프링 설정파일(servlet-context.xml) 작성
5. root-context.xml 작성
6. 컨트롤러와 뷰 작성
7. 실행 

### 스프링 MVC웹 애플리케이션 제작을 위한 폴더 직접 생성

		/프로젝트명/src/main/java
		/프로젝트명/src/main/webapp
		/프로젝트명/src/main/webapp/resources
		/프로젝트명/src/main/webapp/WEB-INF
		/프로젝트명/src/main/webapp/WEB-INF/spring
		/프로젝트명/src/main/webapp/WEB-INF/views

- src와 같은 레벨에 pom.xml 작성 (메이븐 설정파일)

		cf)
		GROUP ID
		group id는 프로젝트마다 구별할 수 있는 고유한 이름이다. 보통은 java의 패키지 네이밍을 따른다. 
		이 규칙이 강제적인 것은 아니다.
		groupid에 많은 하위 group을 만들수 있는데 좋은 방법은 프로젝트 구조로 만드는 것이다. 만약 프로젝트가 멀티 프로젝트가 된다면, 새로운 식별자만 부모의 groupid 뒤에 붙이면 된다.
		ex) org.apache.maven, org.apache.maven.plugins, org.apache.maven.reporting

		ARTIFACT ID		
		artifactid는 jar파일에서 버전 정보를 뺀 이름이다. 
		소문자를 사용하고 이상한 특수문자는 사용하지 않는다.
		ex) maven, commons-math	

- web.xml 작성 -> WEB-INF안에 ->  DispatcherSerlvlet 서블릿 설정, filter설정(인코딩)

- 스프링 설정 파일 (servlet-context.xml) 작성 -> spring/appServlet폴더 안에

- root-context.xml 작성 -> spring폴더 안에

- 컨트롤러와 뷰 작성

<br/><br/>
 
# 17강 - Service & Dao 구현
1. 웹 어플리케이션 준비 
2. 한글 처리
3. 서비스 객체 구현 //실제 업무를 처리할 로직 작업
4. DAO 객체 구현 //DB와의 관계를 이어주는 작업

### STS를 이용해 spring mvc 프로젝트 생성
### 한글처리 (필터를 통한 인코딩)
- web.xml에서 필터설정

``` xml
<filter>
	<filter-name>encodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
	<init-param>
		<param-name>forceEncoding</param-name>
		<param-value>true</param-value>
	</init-param>
</filter>

<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

### 서비스 객체 구현하는 3가지 방법

1. new 연산자를 이용한 service 객체 생성 및 참조 (스프링 X)
```java
MemberService service = new MemberService(); //스프링에선 사용하지 않음
```
2. 스프링 설정파일을 이용한 서비스 객체 생성 생성 및 의존 객체 자동 주입
```xml
<beans: bean id="service" class="~.service.MemberService"></beans:bean>
```
- 자동 DI 주입
```java
@Autowired
MemberSerivce service;
```
3. 어노테이션을 이용해서 서비스 객체 생성 및 의존 객체 자동 주입
``` java
@Service //서비스로 쓰일 객체 명시 (스프링 설정파일에 빈 객체 생성하지 않아도 됨)
//@Component // 같은 기능
//@Repository // 같은 기능
public class MemberService implements IMemberSerivce{
```
- 자동 DI 주입
```java
@Autowired
MemberService service;
```

<br/>
<br/>

# 18강 - Controller 객체 구현
 1. 웹 어플리케이션 준비
 2. @RequestMapping을 이용한 URL 맵핑
 3. 요청 파라미터

 cf) 웹 컴포넌트 (html 파일) vs 뷰 컴포넌트(jsp 파일)

### @RequestMapping을 이용한 URL 맵핑
- 메소드에 @RequestMapping 적용 
: 특정 URL이 들어오면 HandlerAdapter 빈이 메소드를 찾게 해줌

```java
//속성이 하나인 경우, 속성(value) 생략 가능 cf) get/post방식이 맞지 않은 경우에도 value가 같다면 찾아주긴 함.
//@RequestMapping("/memJoin")//GET방식 인 경우 이렇게 생략가능				
@RequestMapping(value="/memJoin", method=RequestMethod.POST)//기본값은 GET
public String memJoin(Model model, HttpServletRequest request){}

```
ex) http://localhost:8090/lec19/memJoin -> memJoin() 실행

- 클래스에 @RequestMapping 적용

cf) 공통된 요청 URL이 중복되는 경우 -> 하나로 처리하는 방법
-> 클래스에 @RequestMapping("상위 루트")를 붙여주기!
```java
@RequestMapping("/member")
public class MemberController{

  //@RequestMapping("/member/memJoin") <-클래스에 @RequestMapping 붙여서 상위 루트 생략 가능
	@RequestMapping("memJoin")
	public String memJoin(){
		//..
```

### 요청 파라미터
: HttpServletRequest 객체를 이용한 HTTP 전송 정보 

``` jsp
ID : <input tupe="text" name = "memId">
PW : <input type="password" name="memPw">
```
- 클라이언트에서 요청(전송)한 HttpReqeust를 자바(서버)에서 받기
- 1. HttpServleRequest객체를 직접 사용한 방법
```java
public String memLogin(HttpServletRequest request){
	String memId = request.getParameter("memId");
	String memPw = request.getParameter("memPw");

	System.out.println(memId, memPw);
}
```
- 2. 어노테이션을 활용한 방법
```java
public String memLogin(@ReuqestParam("memId") String memId, @ReuqestParam("memPw") String memPw){	//어노테이션을 사용해서 받으면서 선언할 수 있음!

	System.out.println(memId, memPw);
```
- 3. required 속성, dafaultValue 속성 사용 -> 값이 넘어오지 않으면 에러를 발생
```java
public String memLogin(@ReuqestParam(value="memId", required="true") String memId,
@ReuqestParam(value"memPw", required=false, defaultValue="1234")){ 
//memId 값이 넘어오지 않으면 에러 
//memPw 값이 넘어오지 않으면 기본값은 "1234"
```

- **4. 커맨드 객체 사용** // setter와 getter를 사용!
```java
public String memLogin(Member member){ //요청param과 Member(모델)객체의 필드명이 일치할 경우 -> setter를 자동으로 호출하여 값을 담아준다.

	System.out.println(member.getMemId(), member.getMemPw());
}
```
-> **view에서 커맨드 객체를 사용**하는 경우 // 변수명을 같게하여 그냥 사용하면 됨!
//컨트롤러 단에서 모델을 사용하지 않아도 뷰 단에서 꺼내쓸 수 있다. ((model.addAttribute() 사용 안함.)

``` jsp
Id : ${member.memId}
Pw : ${member.memPw} 
```

<br/>
<br/>

# 19 강 - Controller 객체 구현2
1. @ModelAttribute
2. 커맨드 객체 프로퍼티 데이터 타입
3. Model & ModelAndView

### @ModelAttribute
- @ModelAttribute를 사용하면 커맨드 객체의 이름을 변경할 수 있고, 이렇게 변견됭 이름은 뷰에서 커맨드 객체를 참조할 때 사용된다. 

``` java
//컨트롤러 단                              // 뷰 단
public String memJoin(Member member)	// ---> ID : ${member.memId}

public String memLogin(Member member)   // ---> ID : ${member.memId}

public String memRemove(@ModelAttribyte("mem") Member member) // ---> ID : ${mem.memId}
```

- @MopdelAttribute가 붙은 메소드는 컨트롤러내의 다른 메소드가 호출되어도 반드시 기본적으로 호출된다. 
== 다른 메소드를 호출하여도 @MopdelAttribute가 붙은 메소드의 데이터를 뷰 단에서 꺼낼 쓸 수 있음.

### 커맨드 객체 프로퍼티 데이터 타입
- 데이터가 기초 데이터 타입인 경우 

		memberJoin.html                                                          Member.java
		</html>									 private String memId;	
		ID : <input type="text" name="memId">					 private String memPw;
		PW : <input type="password" name="memPw">			==>      private String memMail;
		MAIL : <input type="text" name="memMail">				 private int memAge;
		AGE : <input type = "text" name= "membAge" size="4" value="0">

- 데이터가 중첩 커맨드 객체(커맨드 객체 안에서 또다른 커맨드 객체를 사용)를 이용한 List 구조인 경우

		PHONE1: <input type="text" 
		name="memPhones[0].memPhone1" size="5"> - 
		<input type "text" name="memPhones[0].memPhone2" size="5"> -          	 
		<input type="text" name="memPhones[0].memPhone3" size="5">		Member.java
		PHONE2: <input type="text" 					==>     private List<MemPhone> memPhones;
		name="memPhones[1].memPhone1" size="5"> - 
		<input type "text" name="memPhones[2].memPhone2" size="5"> - 
		<input type="text" name="memPhones[3].memPhone3" size="5">


### Model & ModelAndView
- 컨트롤러에서 뷰에 데이터를 전달하기 위해 사용되는 객체로 Model과 ModelAndView가 있다. 두 객체의 차이점은 Model은 뷰에 데이터만을 전달하기 위한 객체고, ModelAndView는 데이터와 뷰의 이름을 함께 전달하는 객체이다. 

```java
public ModelAndView memberModify(Member member){
	Member[] members = service.memberModify(member);

	ModelAndView mav = new ModelAndView();
	mav.addObject("memBef", members[0]);
	mav.addObject("memAft", members[1]);

	mav.setViewName("memModifyOk");

	return mav;
}
```

<br/> 
<br/>

# 20강 - 세션, 쿠키
1. 세션과 쿠키
2. HttpServletRequest를 이용한 세션 사용
3. HttpSession을 이용한 세션 사용
4. 세션 삭제
5. 세션 주요 메소드 및 플로어
6. 쿠키

### 세션과 쿠키
- Connectinoless Protocal
- 웹서비스는 HTTP 프로토콜을 기반으로 하는데, HTTP는 클라이언트와 서버의 관계를 유지하지 않는 특징이 있다.
- 서버의 부하를 줄일 수 있는 장점은 있으나, 매번 새로운 연결이 생성되기 때문에 일반적인 로그인 상태 유지, 장바구니 등의 기능을 구현하기 어렵다.

		클라이언트 -> 요청(Request) : 서버 연결       ->  서버
                  	  <- 응답(Response): 서버 연결 해제 <-

<br/>

- 위의 Connectionless Protocol의 불편함을 해결하기 위해서 세션과 쿠키를 이용.
- 세션과 쿠키는 클라이언트와 서버의 **연결 상태를 유지**하는 방법
- **세션**은 **서버에서 연결정보를 관리**하는 반면, 
- **쿠키**는 **클라이언트에서 연결정보를 관리**하는데 차이가 있다

	클라이언트 -> 1. 로그인 요청 ->           서버 
	  (쿠키)	   -> 2. 로그인 성공 응답 ->    (세션은 서버에서 관리)
 		   -> 3. 상품 주문 요청 ->
		   -> 4. 로그인 유도 페이지 응답 ->

## 세션

	회원정보 수정 또는 삭제 요청 -> 세션에 member속성이 존재하는가? -> NO: 예외 발생 
						|
				YES: 회원정보 수정 또는 삭제 성공 응답	
- 스프링 MVC에서 HttpServletRequest를 이용해서 세션을 이용하려면 컨트롤러의 메소드에서 파라미터로 HttpServletRequest를 받으면 된다.

### HttpServletRequest를 이용한 세션 사용
- 세션을 스프링 컨테이너에게 요청해서 받아오는 개념.
``` java
@RequestMapping(value="/login", method=RequestMethod.POST)
public String memLogin(Member member, HttpServletRequest requests){ //파라미터로 HttpServletRequest받기
	Member mem = service.memberSearch(member);
	HttpSession session = request.getSession(); //HttpSerlvetRequest를 사용해서 세션 생성!!! 
	session.setAttribute("member", mem);

	return "/member/loginOk";
}
``` 

### HttpSession을 이용한 세션 사용
- HttpServletRequest와 HttpSession의 차이점은 거의 없으며, 단지 세션 객체를 얻는 방법에 차이가 있음.
- HttpServletRequest는 getSession()으로 세션 얻음
``` java
HttpSession session = request.getSession();
```

- HttpSession은 메소드의 파라미터로 HttpSession을 받아 세션 사용.
- 스프링에서는 이렇게 사용하는 것 가능 -> 주로 이용하는 방법 
``` java
@RequestMapping(value="/login", method=RequestMethod.POST)
public String memLogin(Member member, HttpSession session){ // 메소드의 파라미터로 HttpSession받기
	Member mem = service.memberSearch(member); 
	session.setAttribute("member", mem); //바로 사용 가능 (세션에 "member"라는 키로 Member 객체 삽입)

	return "/member/loginOk";
}
```

### 세션 삭제  
``` java
session.invalidate();
```

### 세션 사용

``` java
Member member = (Member) session.getAttribute("member");
// 꺼내서 사용하는 개념
```

### 세션 주요 메소드

	getId()	:  세션 ID를 반환.
	setAttribute() : 세션 객체에 속성을 저장
	getAttribute() : 세션 객체에 저장된 속성을 반환
	removeAttribute() : 세션 객체에 저장된 속성을 제거
	setMaxInactiveInterval() : 세션 객체의 유지시간을 설정.
	getMaxInactiveInterval() : 세션 객체의 유지시간을 반환.
	invalidate() : 세션 객체의 모든 정보를 삭제. 
<br/>

## 쿠키
### HttpServletResponse로 쿠키 생성
- 쿠키를 생성할 때는 생성자에 두 개의 파라미터를 넣어주는데 첫 번째는 쿠키 이름을 넣어주고, 두 번째는 쿠키 값을 넣어 준다. 
``` java
@RequestMapping("/main")
public String mallMain(Mall mall, HttpServletResponse response){

Cookie genderCookie = new Cookie("gender", mall.getGender()); //젠더 객체가 담긴 쿠키 생성. 

if(mall.isCookieDel()){ //쿠키가 삭제 되었는지 검사
    genderCookie.setMaxage(0);
    mall.setGender(null);
}else{
    genderCookie.setMaxAge(60*60*24*30); // 1초씩 계산해서 한달동안 쿠키 유지
    response.addCookie(genderCookie) //; HttpServletResponse에 쿠키 담아서 클라이언트에 보내기
    
    return "/mall/main";
}
```

### 쿠키 사용
- 쿠키를 사용할 때는 메소드의 파라미터로@CookieValue를 사용한다. 
``` java
@RequestMapping("/index")
public String mallIndex(Mall mall, @CookieValue(value="gender", required=false) Cookie genderCokkie, HttpServletRequest request){
if(genderCookie != null) mall.setGender(genderCookie.getValue());

return "/mall/index";
}
```
- @CookieValue 어노테이션의 value 속성은 쿠키 이름을 나타낸다. 만약 value에 명시한 쿠키가 없을 경우 예외가 발생한다. required 속성에 false값을 주면 value에 해달하는 쿠키가 없어도 익셉션이 발생하지 않도록 한다.

<br/>
<br/>

# 21강 - 리다이렉트, 인터셉트
1. 리다이렉트
2. 인터셉터

- 컨트롤러에서 뷰를 분기하는 방법과 컨트롤러 실행 전 후에 특정 작업을 가능하게 하는 방법

### 리다이렉트 
- 지금의 페이지에서 특정 페이지로 전환하는 기능

		modifyForm() -> 회원인증 -> NO : return "redirect:/"; (메인 페이지로 유도)
		(회원정보 수정 요청)	  |
				YES : "member/modifyForm";
				(회원정보 수정 페이지로 이동)

### 인터셉터 
- 리다이렉트를 사용해야 하는 경우가 많은 경우 HandlerInterceptor를 이용할 수 있다.
	
		1.Request   
		   ↓
		2.DispatcherServlet
		   ↓
		HandlerIntercepter(Interface)
		3.preHandle() 6.postHandle()   9.afterCompletion()
 		   4.↓             5.↑         	   7.↓ 8. ↑     

				Hander          	          View
				(Controller)	
				   ↓
				 Response

- preHandle() : 주로 쓰임. 컨트롤러가 작업하기 전에 쓰임. 리다이렉트 대체
```java
public class MemberLoginIntercepter extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
				// 이곳에 작성하여 리다이렉션 처리.
		HttpSession session = request.getSession(false); 
		if(session != null) {
			Object obj = session.getAttribute("member");
			if(obj != null) {
				return true;
			}
		}
		
		//세션이 없으면 리다이렉션!
		response.sendRedirect(request.getContextPath()+"/");
		return false;
	}

	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		super.postHandle(request, response, handler, modelAndView);
	}

	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		super.afterCompletion(request, response, handler, ex);
	}
}

```

- 스트링 설정파일에서 <interceptors>에서 설정 
```xml
<interceptors>
	<interceptor>
		<mapping path="/member/modifyForm"/> <!-- 이곳으로 요청 들어오면 -->
		<mapping path="/member/removeForm"/> <!-- 이곳으로 요청 들어오면 -->
		<beans:bean class="com.bs.lec.21.member.MemberLoginInterceptor"/> <!-- 이곳의 인터셉터 사용 -->
	</interceptor>
</interceptors>
```
- 전체를 지정하고 특정을 제외하는 방법도 있음!
```xml
<interceptors>
	<interceptor>
		<mapping path="/member/**"/> <!-- 전체를 받고 -->
		<exclude-mapping path="/member/joinForm"/> <!-- 여기는 제외 -->
		<beans:bean class="com.bs.lec.21.member.MemberLoginInterceptor"/> <!-- 이곳의 인터셉터 사용 -->
	</interceptor>
</interceptors>
```



























 
