## New Maven Project

- Group id : 가장 넓은 프로젝트 이름
- Artifact id : 현재 프로젝트의 이름

## pom.xml 작성
- 필요한 모듈을 가져오기 위한 파일. 
- <dependencies> 
- <build> 
 

## 스프링 프로젝트 구조 
- src/main/java : 자바 파일 관리
- src/main/resources : 자원 파일 관리. 스프링 설정파일(xml) 또는 프로퍼티 파일 등이 관리됨.

## 폴더 및 pom.xml 파일의 이해
- pom.xml 파일은 메이븐 설정 파일로 메이븐은 라이브러리를 연결해주고, 빌드를 위한 플랫폼
- pom.xml에 의해서 필요한 라이브러리를 레퍼지토리에서 다운로드해서 사용.
- cf) 모듈의 라이브러리 파일명은 artifactId + “-“ + 버전명 + “.jar”로 표시됨.

 

