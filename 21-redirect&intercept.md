# 21강 - 리다이렉트, 인터셉트
1. 리다이렉트
2. 인터셉터

- 컨트롤러에서 뷰를 분기하는 방법과 컨트롤러 실행 전 후에 특정 작업을 가능하게 하는 방법

### 리다이렉트 
- 지금의 페이지에서 특정 페이지로 전환하는 기능

		modifyForm() -> 회원인증 -> NO : return "redirect:/"; (메인 페이지로 유도)
		(회원정보 수정 요청)	  |
				YES : "member/modifyForm";
				(회원정보 수정 페이지로 이동)

### 인터셉터 
- 리다이렉트를 사용해야 하는 경우가 많은 경우 HandlerInterceptor를 이용할 수 있다.
	
		1.Request   
		   ↓
		2.DispatcherServlet
		   ↓
		HandlerIntercepter(Interface)
		3.preHandle() 6.postHandle()   9.afterCompletion()
 		   4.↓             5.↑         	   7.↓ 8. ↑     

				Hander          	          View
				(Controller)	
				   ↓
				 Response

- preHandle() : 주로 쓰임. 컨트롤러가 작업하기 전에 쓰임. 리다이렉트 대체
```java
public class MemberLoginIntercepter extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
				// 이곳에 작성하여 리다이렉션 처리.
		HttpSession session = request.getSession(false); 
		if(session != null) {
			Object obj = session.getAttribute("member");
			if(obj != null) {
				return true;
			}
		}
		
		//세션이 없으면 리다이렉션!
		response.sendRedirect(request.getContextPath()+"/");
		return false;
	}

	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		super.postHandle(request, response, handler, modelAndView);
	}

	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		super.afterCompletion(request, response, handler, ex);
	}
}

```

- 스트링 설정파일에서 <interceptors>에서 설정 
```xml
<interceptors>
	<interceptor>
		<mapping path="/member/modifyForm"/> <!-- 이곳으로 요청 들어오면 -->
		<mapping path="/member/removeForm"/> <!-- 이곳으로 요청 들어오면 -->
		<beans:bean class="com.bs.lec.21.member.MemberLoginInterceptor"/> <!-- 이곳의 인터셉터 사용 -->
	</interceptor>
</interceptors>
```
- 전체를 지정하고 특정을 제외하는 방법도 있음!
```xml
<interceptors>
	<interceptor>
		<mapping path="/member/**"/> <!-- 전체를 받고 -->
		<exclude-mapping path="/member/joinForm"/> <!-- 여기는 제외 -->
		<beans:bean class="com.bs.lec.21.member.MemberLoginInterceptor"/> <!-- 이곳의 인터셉터 사용 -->
	</interceptor>
</interceptors>
```
