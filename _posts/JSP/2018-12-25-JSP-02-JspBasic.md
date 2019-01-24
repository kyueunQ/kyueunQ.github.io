## JSP (Java Server Page)

 개발자가 **.jsp** 파일을 만들면, **웹 컨터이너**(서버가 알아서 해주는 부분)가 **_jsp.java** 파일을 만들어 컴파일한 후 **_jsp.class** 파일로 Link한 후 **_jsp.obj** 파일을 만들어 실행한다. *그 결과물로서 **HTML***로 Client에게 응답한다.

- html 파일의 확장자를 jsp로 바꾸면 됨
- html 문법하에 jsp 코드 삽입 
- JSP파일을 수정시 서버를 재실행하지 않아도 되지만, Servlet의 경우 톰캣에서 자동으로 서버를 재실행하여 보여줌



<br>

### JSP 스크립트

1. 선언태그 `<%!   %>`

   : JSP 페이지에서 Java의 **멤버변수 또는 메서드 선언**이 가능

2. 주석태그

   - html: `<!-- -->`
   - jsp: `<%--  --%>`
     - jsp가 .java, .class로 변환될 때 이미 제외되어 페이지 소스보기에서 볼 수 없음  (<!-- -->보임)

3. 스크립트릿(Scriptlet) 태그 `<%   %>`

   : JSP 페이지에서 **java 코드 실행**

4. 표현식 태그 (EL: Expression Language, `<%=  %>`)

   : Java의 **변수 및 메서드의 반환값**을 출력

5. 지시어 `<%@  %>`

   - <%@ page %>
     - 서버에서 jsp 페이지를 처리하는 속성을 정의
   - <%@ include %> 
     - 수 많은 페이지의 공통적인 부분을 적용할 때 사용함 (Ex. header와 footer)
   - <%@ taglib %>
     - 외부 라이브러리를 사용할 때 <%@ taglib uri="  " prefix="네이스페이스명" %>

```jsp
<!-- page 지시어 -->
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>

<!-- taglib 지시어 -->
<%@ taglib url="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<body>
    <!-- include 지시어 -->
    <%@ include file="header.jsp" %>
    
    <!-- 선언 태그 --%>
	<%!
		int x = 10;
		String str = "Hello";
		ArrayList<Integer> list = new ArrayList<Integer>();
		
		public void jspMethod() {
			System.out.println("--jspMethod()--");
		}
	%>
    
	<!-- 스크립트릿 태그 -->
	<% 
		if(x > 0) {
	%>
		<p> x > 0 </p>
	<%
		} else {
	%>
		<p> x <= 0 </p>
	<%
		}
	%>	
	
    <!-- 표현식 태그 -->
	x is <%= x %>
    
    <%@ include file="footer.jsp" %>
</body>
```

<br>

### JSP request와 response

1. request 객체 (요청을 받음)

   *cSignUp.jsp*   : html form 파일을 만들어 데이터를 Client로 부터 받아와 데이터를 처리하는 곳

   ```jsp
   <body>
   	<%!
   	String c_name;
   	String c_pw;
   	String[] c_hobby;
   	%>
   	
   	<%
   	c_name = request.getParameter("c_name");
   	c_pw = request.getParameter("c_pw");
   	c_hobby = request.getParameterValues("c_hobby");
   	%>
   	
   	c_name: <%= c_name%><br>
   	c_pw: <%= c_pw %><br>
   	c_hobby:
   	<%
   		for(int i=0; i<c_hobby.length; i++) {
   	%>
   		<%= c_hobby[i] %>
   	<% 
   		}
   	%> <br>
   	
   	
   	
   </body>
   ```

<br>

1. response 객체 (응답을 보냄)

   *FirstPage.jsp*

   ```jsp
   <body>
   	<h2>FirstPage</h2>
   	<%
   		response.sendRedirect("SecondPage.jsp");
   	%>
   </body>
   ```

<br><br><br>

[[학습자료 출처\] Inflearn, 신입 프로그래머를 위한 실전 JSP](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-jsp_renew/)