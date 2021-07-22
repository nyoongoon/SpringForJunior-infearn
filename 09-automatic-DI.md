## 9강 의존객체 자동 주입

### 의존객체 자동 주입이란?
- 의존 객체 자동 주입
: 스프링 설정 파일에서 의존객체를 주입할 때, 태그를 통해 의존 대상 객체를 명시하지 않아도 스프링 컨테이너가 자동으로 필요한 의존 대상 객체를 찾아서 의존 대상 객체라 필요한 객체에 주입해주는 기능이다. 
: @Autowired와 @Resource 어노테이션을 이용해서 쉽게 구현 가능.


### @Autowired
- 주입하려고 하는 객체의 **타입**이 일치하는 객체를 자동으로 주입한다. 
``` xml
<beans<!-- 네임스페이스, 스키마 설정을 추가-->>

	<context:annotation-config/>
	<!-- 
		기존 객체 주입 설정 삭제
	-->
</beans>

```
- 객체 주입이 필요한 코드 위(생성자, 프로퍼티, 메소드)에 @Autowired 작성.
- 프로퍼티나 메소드에 @Autowired 사용할 때, 반드시 디폴트 생성자를 명시해 주어야함!

### @Resource
- 주입하려고 하는 객체의 **이름**이 일치하는 객체를 자동으로 주입한다. 
- 생성자에는 사용 불가능. 프로퍼티 또는 메소드만!
- 디폴프 생성자 명시해줘야함. 
