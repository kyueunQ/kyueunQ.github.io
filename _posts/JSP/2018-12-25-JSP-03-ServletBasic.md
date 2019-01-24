## Servlet

- 순수 java 파일을 이용하여 만든 Server Side File
- Class 파일을 이용해 클라이언트에게 응답함

<br>

### Servlet Mapping

- 복잡하고 긴 URL을 간단하게 변경함

- [new]-[Daynamic Web Project]를 생성할 때 next 버튼 마지막에서 `Generate web.xml deployment description`을 클릭해 xml 파일을 생성

  - web 환경설정 파일인 `web.xml`이 만들어짐

  ![1545708381314](C:\Users\KyuKaisara\AppData\Roaming\Typora\typora-user-images\1545708381314.png)



1. web.xml 파일을 이용

   *ServletMappingTest.java*

   ```java
   // @WebServlet("/SMTest")
   public class ServletMappingTest extends HttpServlet {
   	// HttpServlet이 추상메서드
   	private static final long serialVersionUID = 1L;
     
     (생략)
     
   	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   		// TODO Auto-generated method stub
   		response.getWriter().append("Served at: ").append(request.getContextPath());
   
   		PrintWriter out = response.getWriter();
   		out.print("<p>Hi Servlet!</p>");
   	
   	}
     (생략)
   ```

   *web.xml*

   ```java
        <servlet>
        	// servlet 이름 설정
        	<servlet-name>servletEx</servlet-name>
        	// package를 포함한 class까지의 풀 네임 입력
        	<servlet-class>com.java.servlet.mapping.ServletMappingTest</servlet-class>
        </servlet>
        
        // Servlet Mapping
        <servlet-mapping>
        	// 어떤 servlet을 Mapping 할 것인지
        	<servlet-name>servletEx</servlet-name>
        	// /SMTest로 매핑할 것이다
        	<url-pattern>/SMTest</url-pattern>
        </servlet-mapping>
   ```

   <br>

1. Java Annotation을 이용

   *ServletMappingTest.java*

   ```java
   @WebServlet("/SMTest2")
   public class ServletMappingTest extends HttpServlet {
   	// HttpServlet이 추상메서드
   	private static final long serialVersionUID = 1L;
     
     (생략)
   ```

   - 위와 동일한 코드에서 맨 위에 @(어노테이션)을 추가함
   - 앞서 설정한 ServletMapping을 통해 `http://localhost:8090/ServletMappingEx/SMTest`로 접속이 가능하며, @을 통해 설정한 `http://localhost:8090/ServletMappingEx/SMTest2` 로도 접근이 가능하다.



> - ServletMapping을 하지 않는다면 ,`http://localhost:8090/ServletMappingEx/com.java.servlet.mapping.ServletMappingTest`
>
>   `protocol//DNS:port/Daynamic Web Pjt Name/package.className`
>
>    ***길고 복잡한 URL, 보안에 취약함***
>
> - 반면 ServletMapping을 한다면
>
>   `http://localhost:8090/ServletMappingEx/SMTest`
>
>    *간결한 URL로 지정할 수 있음*

<br>

### Servlet request와 response

- HttpServlet이란 추상 메서드를 상속하여 사용함

```java
@WebServlet("/ServletTestClass")
public class ServletTestClass extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public ServletTestClass() {
        super();
        // TODO Auto-generated constructor stub
    }
    
    // Get방식
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
        
        // HttpServletRequest (사용자 → 서블릿)
        
        // 쿠키, 세션 정보 가져오기
        request.getCookies();
        request.getSession();
        // 속성 정보 출력 및 수정
        request.getAttribute(null);
        request.setAttribute(null, null);
        // form의 이름, 취미, 주소 등 정보 추출
        request.getParameter(null);
        request.getParameterNames();
        request.getParameterValues(null);
        
       // HttpServletResponse (서블릿 → 사용자)
       // 서버가 브라우저에 데이터를 줄 때
        response.addCookie(null);
        response.getStatus();
        response.sendRedirect(null);
        response.getWriter();
        response.getOutputStream();
	
    }

    // Post방식
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
```

<br>

### Servlet Life Cycle

- 클라이언트의 요청에 의해 Servlete이 생성( init( ) ), 실행( service ), 소멸( destroy( ) )되기까지 웹 컨테이너에서 알아서 처리함
- @PostConstruct (Servlet 생성 단계) init( ) 전에 발생
- @PreDestroy (servlet 소멸 단계) destroy( ) 이후 발생
- 필요에 따라 doGet (service 단계)와 @PostConstruct만 생성해도 됨

```java
@WebServlet("/ServletTestClass")
public class ServletTestClass extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public ServletTestClass() {
        super();
    }
    
    
    // Get방식
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
		System.out.println("--doGet()--");
		
    }
    
    // Post방식
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
	
	@PostConstruct
	private void fucPc() {
		System.out.println("--@PostConstruct--");
	}
	
	@Override
	public void init() throws ServletException {
		System.out.println("--init( )--");
	}
	
	@Override
	public void destroy() {
		System.out.println("--destroy( )--");
	}
	
	@PreDestroy
	private void funPd() {
		System.out.println("--@PreDestroy --");
	}

}
```

<br><br><br>

 [[학습자료 출처\] Inflearn, 신입 프로그래머를 위한 실전 JSP](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-jsp_renew/)