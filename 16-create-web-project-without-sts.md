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

 
