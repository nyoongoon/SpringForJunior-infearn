# 19 강 - Controller 객체 구현2
1. @ModelAttribute
2. 커맨드 객체 프로퍼티 데이터 타입
3. Model & ModelAndView

### @ModelAttribute
- @ModelAttribute를 사용하면 커맨드 객체의 이름을 변경할 수 있고, 이렇게 변견됭 이름은 뷰에서 커맨드 객체를 참조할 때 사용된다. 

``` java
//컨트롤러 단                              // 뷰 단
public String memJoin(Member member)	// ---> ID : ${member.memId}

public String memLogin(Member member)   // ---> ID : ${member.memId}

public String memRemove(@ModelAttribyte("mem") Member member) // ---> ID : ${mem.memId}
```

- @MopdelAttribute가 붙은 메소드는 컨트롤러내의 다른 메소드가 호출되어도 반드시 기본적으로 호출된다. 
== 다른 메소드를 호출하여도 @MopdelAttribute가 붙은 메소드의 데이터를 뷰 단에서 꺼낼 쓸 수 있음.

### 커맨드 객체 프로퍼티 데이터 타입
- 데이터가 기초 데이터 타입인 경우 

		memberJoin.html                                                          Member.java
		</html>																	 private String memId;	
		ID : <input type="text" name="memId">								     private String memPw;
		PW : <input type="password" name="memPw">						==>      private String memMail;
		MAIL : <input type="text" name="memMail">								 private int memAge;
		AGE : <input type = "text" name= "membAge" size="4" value="0">

- 데이터가 중첩 커맨드 객체(커맨드 객체 안에서 또다른 커맨드 객체를 사용)를 이용한 List 구조인 경우

		PHONE1: <input type="text" 
		name="memPhones[0].memPhone1" size="5"> - 
		<input type "text" name="memPhones[0].memPhone2" size="5"> -          	 
		<input type="text" name="memPhones[0].memPhone3" size="5">				Member.java
		PHONE2: <input type="text" 										==>     private List<MemPhone> memPhones;
		name="memPhones[1].memPhone1" size="5"> - 
		<input type "text" name="memPhones[2].memPhone2" size="5"> - 
		<input type="text" name="memPhones[3].memPhone3" size="5">


### Model & ModelAndView
- 컨트롤러에서 뷰에 데이터를 전달하기 위해 사용되는 객체로 Model과 ModelAndView가 있다. 두 객체의 차이점은 Model은 뷰에 데이터만을 전달하기 위한 객체고, ModelAndView는 데이터와 뷰의 이름을 함께 전달하는 객체이다. 

```java
public ModelAndView memberModify(Member member){
	Member[] members = service.memberModify(member);

	ModelAndView mav = new ModelAndView();
	mav.addObject("memBef", members[0]);
	mav.addObject("memAft", members[1]);

	mav.setViewName("memModifyOk");

	return mav;
}
```
