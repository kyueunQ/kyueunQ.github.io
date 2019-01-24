# 한글처리

- 영어는 1byte, 한글은 2byte

- UTF-8 : 만국 공용어

  <br>

1. Post/Get 전송 방식에 따른 한글처리 방법
2. 인터페이스 Filter

<br><br>

## 1. 전송 방식에 따른 한글처4리

1. Post 방식
   - 해당 서블릿마다  `request.setCharacterEncoding("UTF-8");`을 삽입
2. Get 방식
   - 사용하는 서버의 *server.xml*에 `URIEncoding="UTF-8"`만 추가

<br>

### Post 방식

- JSP → Servlet과 JSP → JSP 의 2가지 경우

<br>

#### (1) JSP → Servlet

*formEx.jsp*

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>  
	
(중략)

<body>
	<form action="mSignUp" method="post">
		이름: <input type="text" name="m_name"><br>
		별명: <input type="text" name="m_nickname"><br>
		<input type="submit" value="sign up">
	
	</form>

</body>
```

- [Window] - [Preferences] - 좌측 상단 검색창에 'Encoding' 검색시 나오는 항목들에서 UTF-8로 apply 해뒀을 경우 자동으로 언어 설정이 변경되어 보임

  <br>

*mSignUp.java*

```java
@WebServlet("/mSignUp")
public class mSignUp extends HttpServlet {

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		request.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		String mName = request.getParameter("m_name");
		String mNickName = request.getParameter("m_nickname");
		
		out.print(mName);
		out.print(mNickName);
				
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```

<br>

#### (2) JSP  → JSP 페이지

*formEx.jsp*

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>  
	
(중략)

<body>
	<form action="mSignUp.jsp" method="post">
		이름: <input type="text" name="m_name"><br>
		별명: <input type="text" name="m_nickname"><br>
		<input type="submit" value="sign up">
	
	</form>

</body>
```

- `<form action="mSignUp" method="post">`라면 mSignUp.java 서블릿으로 연결되기 때문에 `<form action="mSignUp.jsp" method="post">` mSignUp.jsp를 꼭 붙여주기

<br>

*mSignUp.jsp*

```jsp
<% request.setCharacterEncoding("UTF-8"); %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

(중략)

<body>
	
	<%!
		String mName;
		String mNickname;
	%>
	
	<%
		mName = request.getParameter("m_name");
		mNickname = request.getParameter("m_nickname");
	%>
	
	이름: <%= mName %><br>
	별명: <%= mNickname %>
</body>
</html>
```

- 맨 위에 `<% request.setCharacterEncoding("UTF-8"); %>`를 추가해줘야 웹 상에서 한글 출력이 온전히 이뤄짐
  - 위의 *mSignUp.java*에서 `request.setCharacterEncoding("UTF-8");`의 역할

<br><br>

### Get 방식

- 사용하는 서버의 *server.xml*에서 Ctrl + F 로 'Connector' 검색하기

  *server.xml*

  ```xml
  <Connector URIEncoding="UTF-8" connectionTimeout="20000" port="8090" protocol="HTTP/1.1" redirectPort="8443"/>
  ```

  - `<Connector ` 뒤에 `URIEncoding="UTF-8" `추가하기

- 만약 서버가 구동 중이였다면 정지 버튼을 누른 후, Servers창에서 해당 서버를 클릭하고 우측 상단의 맨 오른쪽의 동기화 버튼을 클릭하면 [Stopped, Synchronized] 표시가 뜨면 됨

<br><Br><br>

## 2. Filter (Chain기법)

- 그런데 위의 말대로라면 post 방식을 사용할 때면 번거롭게 매번 request.setCharacterEncoding을 넣어줘야할까?

  - 이러한 비효율적인 방법을 **Filter**로 처리할 수 있음

- Filter는 interface이며, 구현을 위해 Class를 생성해야 됨

  <br>

### Filter 사용법

(1) Filter interface를 구현한 Class를 생성

(2) web.xml에서 Filter를 모든 페이지에서 적용할 수 있도록 Mapping 해주기

<br>

*EncodingFilter.java*

```java
package com.encoding.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
 

public class EncodingFilter implements Filter{
	
	@Override 
	public void init(FilterConfig arg0) throws ServletException {
		System.out.println("----Filter init()----");
	}
	
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		
		System.out.println("----Filter doFilter()----");
		
		request.setCharacterEncoding("UTF-8");
		
		chain.doFilter(request, response);
	}
	
	@Override
	public void destroy() {
		System.out.println("----Filter destroy()----");
	}

}
```

- `import javax.servlet.Filter;` 서블릿 Filter을 import하면 *EncodingFilter*에 빨간 밑줄이 생긴다. 이때 마우스를 살짝 가져자 대고 'Add unimplemented methods'를 클릭하면 필요한 init( ), doFilter( ), destroy( )  메서드가 자동으로 생성된다.

<br>

*web.xml*

```xml
<filter>
  	<filter-name>encodingFilter</filter-name>
  	<filter-class>com.encoding.filter.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>encodingFilter</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

- /* : 서버에 들어오는 모든 요청에 대해 해당 필터를 거치도록 설정함