# 18강 - Controller 객체 구현
 1. 웹 어플리케이션 준비
 2. @RequestMapping을 이용한 URL 맵핑
 3. 요청 파라미터

 cf) 웹 컴포넌트 (html 파일) vs 뷰 컴포넌트(jsp 파일)

### @RequestMapping을 이용한 URL 맵핑
- 메소드에 @RequestMapping 적용 
: 특정 URL이 들어오면 HandlerAdapter 빈이 메소드를 찾게 해줌

```java
//속성이 하나인 경우, 속성(value) 생략 가능 cf) get/post방식이 맞지 않은 경우에도 value가 같다면 찾아주긴 함.
//@RequestMapping("/memJoin")//GET방식 인 경우 이렇게 생략가능				
@RequestMapping(value="/memJoin", method=RequestMethod.POST)//기본값은 GET
public String memJoin(Model model, HttpServletRequest request){}

```
ex) http://localhost:8090/lec19/memJoin -> memJoin() 실행

- 클래스에 @RequestMapping 적용

cf) 공통된 요청 URL이 중복되는 경우 -> 하나로 처리하는 방법
-> 클래스에 @RequestMapping("상위 루트")를 붙여주기!
```java
@RequestMapping("/member")
public class MemberController{

  //@RequestMapping("/member/memJoin") <-클래스에 @RequestMapping 붙여서 상위 루트 생략 가능
	@RequestMapping("memJoin")
	public String memJoin(){
		//..
```

### 요청 파라미터
: HttpServletRequest 객체를 이용한 HTTP 전송 정보 

``` jsp
ID : <input tupe="text" name = "memId">
PW : <input type="password" name="memPw">
```
- 클라이언트에서 요청(전송)한 HttpReqeust를 자바(서버)에서 받기
- 1. HttpServleRequest객체를 직접 사용한 방법
```java
public String memLogin(HttpServletRequest request){
	String memId = request.getParameter("memId");
	String memPw = request.getParameter("memPw");

	System.out.println(memId, memPw);
}
```
- 2. 어노테이션을 활용한 방법
```java
public String memLogin(@ReuqestParam("memId") String memId, @ReuqestParam("memPw") String memPw){	//어노테이션을 사용해서 받으면서 선언할 수 있음!

	System.out.println(memId, memPw);
```
- 3. required 속성, dafaultValue 속성 사용 -> 값이 넘어오지 않으면 에러를 발생
```java
public String memLogin(@ReuqestParam(value="memId", required="true") String memId,
@ReuqestParam(value"memPw", required=false, defaultValue="1234")){ 
//memId 값이 넘어오지 않으면 에러 
//memPw 값이 넘어오지 않으면 기본값은 "1234"
```

- **4. 커맨드 객체 사용** // setter와 getter를 사용!
```java
public String memLogin(Member member){ //요청param과 Member(모델)객체의 필드명이 일치할 경우 -> setter를 자동으로 호출하여 값을 담아준다.

	System.out.println(member.getMemId(), member.getMemPw());
}
```
-> **view에서 커맨드 객체를 사용**하는 경우 // 변수명을 같게하여 그냥 사용하면 됨!
//컨트롤러 단에서 모델을 사용하지 않아도 뷰 단에서 꺼내쓸 수 있다. ((model.addAttribute() 사용 안함.)

``` jsp
Id : ${member.memId}
Pw : ${member.memPw} 
```

















