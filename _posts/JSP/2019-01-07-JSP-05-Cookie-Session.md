# Cookie와 Session

http특성 연결을 유지시켜주는 수단으로 사용되는 2가지

1. Cookie
2. Session



## 1. Cookie

- 프로그램에 서버와 클라이언트가 연결을 시도한 흔적을 남김



브라우저와 서버가 연결 

http 프로토콜: 한 번 요청과 한 번 응답 후 브라우저와 서버의 연결을 해지함자원을 낭비하지 않기 위해 서버와의 연결을 바로 해지함



하지만 장바구니를 생각하면 누가 담았는지 알 수 없는 문제가



데이터가 바로 사라져버림

로그인의 경우 다른 페이지로 이동했는데 바로 연결이 끊겨서는 안돼 



그래서 쿠키를 이용해 서버와 브라우저 간의 데이터를 연결시켜주자

////

요청과 응답을 한 정보를 서버에 저장하기에는 부하 위험이 있기에

해당 브라우저에 '기존 연결정보 저장'을 쿠키에 함





(1) cooki == null? 인지 확인

(2) null이라면 쿠키를 생상하고,  null이 아니라면 기존의 쿠키를 재활용함

(3) 해당 쿠키 정보를 다시 클라이언트에 response로 보냄



로그인 -> action form을 통해 loginCon.java 서블릿으로 이동 -> 

쿠키 존재 유무를 확인함 -> loginOk.jsp에서 해당 쿠키 정보를 출력해 본다.



브라우저가 만든 자체적인 로그가 있어서 

`cookies: [Ljavax.servlet.http.Cookie;@69b73435`

쿠키가 객체여서 맨끝에 해당 객체의 주소 값을 보여줌



클라이언트가 만들고 클라이언터 브라우저에 저장된다.

- 사용자의 정보가 로컬에 저장되기 때문에 정보 유출의 위험으로 보안에 취약함



<br><br>

## Session

- 브라워와 서버의 연결을 유지
- 쿠키는 브라우저에서 생산 저장
- 세션은 웹 컨테이너에서 생성되어 서버에 저장됨
- 둘 다 http 프로토콜을 사용하기 때문에 연결이 해지됨 재연결 가능



### Session 구현 방법

(1) Session == null?

(4) 로그아웃하면 해당 정보들을 지움
































































































































































































































































































