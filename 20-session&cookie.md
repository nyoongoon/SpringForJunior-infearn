# 20강 - 세션, 쿠키
1. 세션과 쿠키
2. HttpServletRequest를 이용한 세션 사용
3. HttpSession을 이용한 세션 사용
4. 세션 삭제
5. 세션 주요 메소드 및 플로어
6. 쿠키

### 세션과 쿠키
- Connectinoless Protocal
- 웹서비스는 HTTP 프로토콜을 기반으로 하는데, HTTP는 클라이언트와 서버의 관계를 유지하지 않는 특징이 있다.
- 서버의 부하를 줄일 수 있는 장점은 있으나, 매번 새로운 연결이 생성되기 때문에 일반적인 로그인 상태 유지, 장바구니 등의 기능을 구현하기 어렵다.

		클라이언트 -> 요청(Request) : 서버 연결       ->  서버
                  	  <- 응답(Response): 서버 연결 해제 <-

<br/>

- 위의 Connectionless Protocol의 불편함을 해결하기 위해서 세션과 쿠키를 이용.
- 세션과 쿠키는 클라이언트와 서버의 **연결 상태를 유지**하는 방법
- **세션**은 **서버에서 연결정보를 관리**하는 반면, 
- **쿠키**는 **클라이언트에서 연결정보를 관리**하는데 차이가 있다

	클라이언트 -> 1. 로그인 요청 ->           서버 
	  (쿠키)	   -> 2. 로그인 성공 응답 ->    (세션은 서버에서 관리)
 		   -> 3. 상품 주문 요청 ->
		   -> 4. 로그인 유도 페이지 응답 ->

## 세션

	회원정보 수정 또는 삭제 요청 -> 세션에 member속성이 존재하는가? -> NO: 예외 발생 
						|
				YES: 회원정보 수정 또는 삭제 성공 응답	
- 스프링 MVC에서 HttpServletRequest를 이용해서 세션을 이용하려면 컨트롤러의 메소드에서 파라미터로 HttpServletRequest를 받으면 된다.

### HttpServletRequest를 이용한 세션 사용
- 세션을 스프링 컨테이너에게 요청해서 받아오는 개념.

``` java
@RequestMapping(value="/login", method=RequestMethod.POST)
public String memLogin(Member member, HttpServletRequest requests){ //파라미터로 HttpServletRequest받기
	Member mem = service.memberSearch(member);
	HttpSession session = request.getSession(); //HttpSerlvetRequest를 사용해서 세션 생성!!! 
	session.setAttribute("member", mem);

	return "/member/loginOk";
}
``` 

### HttpSession을 이용한 세션 사용
- HttpServletRequest와 HttpSession의 차이점은 거의 없으며, 단지 세션 객체를 얻는 방법에 차이가 있음.
- HttpServletRequest는 getSession()으로 세션 얻음

``` java
HttpSession session = request.getSession();
```

- HttpSession은 메소드의 파라미터로 HttpSession을 받아 세션 사용.
- 스프링에서는 이렇게 사용하는 것 가능 -> 주로 이용하는 방법 

``` java
@RequestMapping(value="/login", method=RequestMethod.POST)
public String memLogin(Member member, HttpSession session){ // 메소드의 파라미터로 HttpSession받기
	Member mem = service.memberSearch(member); 
	session.setAttribute("member", mem); //바로 사용 가능 (세션에 "member"라는 키로 Member 객체 삽입)

	return "/member/loginOk";
}
```

### 세션 삭제  
``` java
session.invalidate();
```

### 세션 사용

``` java
Member member = (Member) session.getAttribute("member");
// 꺼내서 사용하는 개념
```

### 세션 주요 메소드

	getId()	:  세션 ID를 반환.
	setAttribute() : 세션 객체에 속성을 저장
	getAttribute() : 세션 객체에 저장된 속성을 반환
	removeAttribute() : 세션 객체에 저장된 속성을 제거
	setMaxInactiveInterval() : 세션 객체의 유지시간을 설정.
	getMaxInactiveInterval() : 세션 객체의 유지시간을 반환.
	invalidate() : 세션 객체의 모든 정보를 삭제. 
<br/>
## 쿠키

### HttpServletResponse로 쿠키 생성
- 쿠키를 생성할 때는 생성자에 두 개의 파라미터를 넣어주는데 첫 번째는 쿠키 이름을 넣어주고, 두 번째는 쿠키 값을 넣어 준다. 
``` java
@RequestMapping("/main")
public String mallMain(Mall mall, HttpServletResponse response){

Cookie genderCookie = new Cookie("gender", mall.getGender()); //젠더 객체가 담긴 쿠키 생성. 

if(mall.isCookieDel()){ //쿠키가 삭제 되었는지 검사
    genderCookie.setMaxage(0);
    mall.setGender(null);
}else{
    genderCookie.setMaxAge(60*60*24*30); // 1초씩 계산해서 한달동안 쿠키 유지
    response.addCookie(genderCookie) //; HttpServletResponse에 쿠키 담아서 클라이언트에 보내기
    
    return "/mall/main";
}
```

### 쿠키 사용
- 쿠키를 사용할 때는 메소드의 파라미터로@CookieValue를 사용한다. 
``` java
@RequestMapping("/index")
public String mallIndex(Mall mall, @CookieValue(value="gender", required=false) Cookie genderCokkie, HttpServletRequest request){
if(genderCookie != null) mall.setGender(genderCookie.getValue());

return "/mall/index";
}
```
- @CookieValue 어노테이션의 value 속성은 쿠키 이름을 나타낸다. 만약 value에 명시한 쿠키가 없을 경우 예외가 발생한다. required 속성에 false값을 주면 value에 해달하는 쿠키가 없어도 익셉션이 발생하지 않도록 한다.
