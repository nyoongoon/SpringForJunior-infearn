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






























 
