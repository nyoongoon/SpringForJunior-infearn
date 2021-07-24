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
 
		    	HandlerMapping	 HandlerAdapter <-> Controller (<-> Service <-> DAO <-> Model <-> DB)
					 /           /            (요청처리)
		브라우저 -> DispathcerServlet 
			↑(응답)	 \(응답생성)	 \(처리결과를 출력할 view선택)
			-------- View		ViewResolver


- 1. 브라우저 -(HttpRequest)-> DispatcherServlet -> HandlerMapping : 알맞은 컨트롤러 선택 -> DispathcerServlet
- 2. DispatcherServlet -> HandlerAdapter : 선택된 컨트롤러에서 적합한 메소드를 찾아서 컨트롤러&메소드 호출 -> model&view를 받아서 DispatcherServlet반환 
- 3. DispatcherSerlvet -> ViewResolver : 받아온 model&view을 사용할 jsp문서 선택 -> DispatcherSerlvet 
- 4. DispatcherSerlvet -> View(jsp) -(HttpResponse)-> 브라우저 

### DispatcherSerlvet 설정 (프레임워크가 생성)
- web.xml에 서블릿을 매핑
``` xml
<servlet>
	<servlet-name>appServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
	</init-param>								<!-- ㄴ> 스프링 설정파일 -->		
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
    public String success(**Model model**){}
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
- -> JSP파일경로 : /WEB-INF/views/success.jsp
									↑(컨트롤러에서)
						public String success(Model model){
							return "success";
						}
