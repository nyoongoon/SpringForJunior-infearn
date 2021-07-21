# 8강 스프링 설정 파일 분리 // 빈의 범위
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
: 싱글톤과 반대대는 개념으로 스프링 설정파일에서 Bean객체를 정의할 때 scope속성을 명시해주면 된다.
```xml
<bean id = "~" class = "~" scope="prototype">
</bean>
```
