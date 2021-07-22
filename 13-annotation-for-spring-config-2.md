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




