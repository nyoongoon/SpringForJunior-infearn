# 10강 - 의존 객체 선택
1. 의존 객체 선택
2. 의존 객체 자동 주입 체크
3. @Inject
<br/>
- 의존 객체가 자동으로 주입될 때 스프링 컨테이너에서 어떻게 의존 객체가 선택되는지 .
- 다수의 빈 객체 중 의존 객체의 대상이 되는 객체를 선택하는 방법. 
<br/>

### 의존 객체 선택
- 동일한 객체가 2개 이상인 경우 스프링 컨테이너는 자동 주입 대상 객체를 판단하지 못해서 예외를 발생시킨다. 
- -> qualifier 태그 사용
- xml파일에서
```xml
<bean id ="~" class="~">
	<qualifier value ="usedDao"/>
</bean>
```
- java파일에서
```java
@Autowired
@Qualifier("usedDao");
private WordDao wordDao;
```

### 의존 객체 자동 주입 체크 (잘 안씀)
- 스프링 설정파일에 주입해야할 빈 파일이 설정 안 된 경우.
```java
 @Autowired(required = false)
```
- -> 없으면 예외 발생시키지 말고 그냥 주입하지 마라.

### @Inject
- @Autowired와 거의 비슷하게 @Inject 어노테이션을 이용해서 의존 객체를 자동으로 주입할 수 있다.
- @Inject은 required 속성을 지원하지 않는다. 
cf) @Autowired가 더 많이 쓰임
- @Named : @Qualifier와 비슷한 기능
```xml
<bean id="wordDao1" class = "~.WordDao"/>
```
``` java
@Inject
@Named(value="wordDao1")
private WordDao wordDao
```
