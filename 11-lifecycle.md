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
			afterPropertiesSet()	destroy()
		       		 ↑					 ↑
						<빈(Bean) 객체>
						afterPropertiesSet() <- 빈객체 생성 시점에 호출
						destroy()	<- 빈객체 소멸 시점에 호출

- 자바 파일에서 -> InitializingBean // DisposableBean 인터페이스 구현
-> 빈 객체가 생성 될 때 특정 작업을 작성 -> afterPropertiesSet()
-> 빈 객체가 소멸 될 때 특정 작업을 작성 -> destroy()

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
