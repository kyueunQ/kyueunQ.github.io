# JSP 내장 객체

- Ex. Request와 Response

<br>

## 1. config 객체

- 데이터 공유 (해당 servlet에서만 가능)
- web.xml에 데이터를 명시하고 `getinitParameter( "name");`으로 데이터를 공유함

*web.xml*

```xml
<servlet>
  	<servlet-name>servletEx</servlet-name>
  	<jsp-file>/JspEx.jsp</jsp-file>
    // 공유할 데이터명과 값을 입력
  	<init-param> 
  		<param-name>adminId</param-name>
  		<param-value>admin</param-value>
  	</init-param>
  	<init-param>
  		<param-name>adminPw</param-name>
  		<param-value>1234</param-value>
  	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>servletEx</servlet-name>
 	<url-pattern>/JspEx.jsp</url-pattern>
</servlet-mapping>
```



*jspEx.jsp*

```jsp
<body>
	<%!
	String adminId;
	String adminPw;
	%>
	
	<%
	adminId = config.getInitParameter("adminId");
	adminPw = config.getInitParameter("adminPw");
	%>
    
    <!--
	String adminId =
	getServletConfig().getinitParameter("adminId");
	위와 동일한 의미
   	-->
	
	<p>adminId : <%= adminId %></p>
	<p>adminPw : <%= adminPw %></p>

</body>
```

<br>

## 2. application 객체

- 데이터 공유 (어플리케이션 **전체에서 공유 가능**토록 함)



1. `application.getInitParameter( );`

   *web.xml*

   ```xml
   <context-param>  
       // Client가 이미지를 요청했을 때
     	<param-name>imgUp</param-name>
     	<param-value>/upload/img</param-value>
   </context-param>
   <context-param>
     	<param-name>testServerIP</param-name>
     	<param-value>127.0.0.1</param-value>
   </context-param>
   ```

   *jspEx.jsp*

   ```jsp
   <%
   	imgDir = application.getInitParameter("imgDir");
   	testServerIP = application.getInitParameter("testServerIP");
   	%>
   	<p>imgDir : <%= imgDir %></p>
   	<p>testServerIP : <%= testServerIP %></p>
   ```

   <br>

2.  `application.setAttribute( );`

   `application.getAttribute( );`

   *JspEx.jsp*

   ```jsp
   <%
   	application.setAttribute("A", "abcd");
   	application.setAttribute("B", "haha");
   	%>
   ```

   *getAttributeTest.jsp*

   ```jsp
   <body>
   
   	<%!
   	String A;
   	String B;
   	%>
   	
   	<%
   	A = (String)application.getAttribute("A");
   	B = (String)application.getAttribute("B");
   	%>
   	
   	<p>A: <%= A %></p>
   	<p>B: <%= B %></p>
   
   
   </body>
   ```

<br>

## 3. Exception 객체

- 에러가 발생시 공통적으로 보여줄 화면을 설정할 때 사용

  *JspEx.jsp*

  ```jsp
  <!--지시어를 통해 명시-->
  <%@ page errorPage="errPage.jsp" %>
  ```

  *errPage.jsp*

  ```jsp
  <%@ page isErrorPage="true" %>
  ...
  <body>
  
  	<%
  	response.setStatus(200);
  	String msg = exception.getMessage();
  	%>
  	
  	<h3>error : <%= msg %> </h3>
  
  </body>
  ```

<br><br><br>

# Servlet 데이터 공유

- 위의 JSP 내장 객체 사용 방법과 유사함

<br>

## 1. servlet parameter

- getServletConfig().getInitParameter();

- web.xml의 경우 위의 JSP 때와 동일함

  *web.xml*

  ```xml
  <servlet>
    	<servlet-name>servletEx</servlet-name>
    	<jsp-file>/JspEx.jsp</jsp-file>
      // 공유할 데이터명과 값을 입력
    	<init-param> 
    		<param-name>adminId</param-name>
    		<param-value>admin</param-value>
    	</init-param>
    	<init-param>
    		<param-name>adminPw</param-name>
    		<param-value>1234</param-value>
    	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>servletEx</servlet-name>
   	<url-pattern>/JspEx.jsp</url-pattern>
  </servlet-mapping>
  ```

  *ServletEx.java*

  ```java
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  		String userId = getServletConfig().getInitParameter("userId");
  		String userPw = getServletConfig().getInitParameter("userPw");
  	
  		PrintWriter out = response.getWriter();
  		out.print(userId);
  		out.print(userPw);
  		
  	}
  ```

<br>

## 2. context parameter

- getServletContext().getInitParameter(");

- web.xml의 경우 위의 JSP 때와 동일함

  *web.xml*

  ```xml
  <context-param>  
      // Client가 이미지를 요청했을 때
    	<param-name>imgUp</param-name>
    	<param-value>/upload/img</param-value>
  </context-param>
  <context-param>
    	<param-name>testServerIP</param-name>
    	<param-value>127.0.0.1</param-value>
  </context-param>
  ```

  *ServletEx.java*

  ```java
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  		
  		String imgDir = getServletContext().getInitParameter("imgDir");
  		String testServerIP = getServletContext().getInitParameter("testServerIP");
  	}
  ```

<br>

## 3. context attribute

- setAttribute( ); 와 getAttribute( );

  *ServletEx.java*

  ```java
  @WebServlet("/ServletEx")
  public class ServletEx extends HttpServlet {
  	private static final long serialVersionUID = 1L;
         
  	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  		getServletContext().setAttribute("C", "ABC");
  		getServletContext().setAttribute("D", "EFG");
  	
  	}
  ```

  *getServlet.java*

  ```java
  @WebServlet("/getservlet")
  public class getServlet extends HttpServlet {
  	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  
  		String C = (String)getServletContext().getAttribute("C");
  		String D = (String)getServletContext().getAttribute("D");
  
  		PrintWriter out = response.getWriter();
  		out.print(C);
  		out.print(D);
  	}
  
  	
  ```

  - 이번에는 web.xml에서 아닌 @을 사용해서 url 설정

