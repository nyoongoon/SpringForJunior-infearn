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


