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

