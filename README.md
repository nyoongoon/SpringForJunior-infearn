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

 

