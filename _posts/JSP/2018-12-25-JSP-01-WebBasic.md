---

layout: post
title: [JSP day01] Web Basic 
feature-img: "assets/img/sample.png"                
tags: [jsp, web]

---

# JSP & Servlet

: java를 기반으로 웹 프로그램을 만들 때 사용하는 언어

- JSP : View를 만들 때 사용
- Servlet: Controller와 Model을 만들 때 사용

<br>

## 웹 프로그램

: 브라우저에서 정보 요청 (request 데이터 수집, 가공)하고, 서버에서 만든 데이터를 응답 (response)하는 과정을 만드는 것이 웹 프로그래밍 개발  

<br>

### Protocol

: 통신을 할 떄 필요한 규약을 *Protocol* 이라고 말함

 *Protocol*에는 다음과 같은 것들이 있음

- HTTP: 하이퍼링크를 이용해 텍스트, 이미지, 영상 등으로 응답
- FTP: 파일 자체가 전송. 파일 업로드와 내려받기 기능
- SMTP: 메일을 주고 받는 프로토콜
- POP: 

### IP (Internet Protocol)

- 한 컴퓨터에서 특정한 주소를 부여함
- IPv4 = 4byte 인터넷 주소 (255.255.255.255)
- IPv6 = 16byte IPv4 고갈에 대한 대안

### DNS (Domain Name System)

- 클라이언트 → **DNS** → IP주소 → 웹 서버에 접근 → 특정 프로그램에 접근
- IP주소를 다 외울 수 없기에 naver.com, google.com처럼 이름을 붙여 편리하게 대체함

### Port

- 이때 구동되는 프로그램이 여러개 있는데, 특정 프로그램을 찾아가지 위한 경로를 *port* 라고 함
- 웹 서비스의 경우 80, 암호화된 통신(https)는 443번

```
http://www.google.com:80/index.html
 ①			② 		  ③		④

 ① Protocol
 ② Domain
 ③ Port
 ④ 연결된 서버의 특정 프로그램 경로
```

<br>

### 웹 프로그램 동작 원리

- User/Client(Browser) → <Request> → Web Server → <query>  → Database
- Database → <result> → Web Server → <response> → User/Client(Browser)
- 이때 Web Server에서 정적 데이터를 응답하기 위해 *HTML*을 사용하고, 추출한 데이터를 새롭게 가공하고 제작한다면 *Web Container*를 사용하고 결과적으로 HTML을 통해 동적 데이터를 전송함

<br>

<br>

## 개발환경 세팅하기

1. JDK (Java Development Kit) 설치
   - jre는 프로그램이 구동되는데 필요한데 개발까지 해야한다면 jdk를 설치해야 함
2. 환경변수 설정 (path 설정)
   - `java.exe` : JVM 작동 
   - `javac.exe` : 컴파일러 
   - `JAVA_HOME`과 `Path` 경로를 설정해주기
3. 이클립스 설치
4. 웹 컨테이너 설치
   - ApacheTomcat 8.5 (jsp 2.3/ Servlet 3.1 )

<br><br>

### 웹 컨테이너 (tomcat)

- 개발자가 만든 .jsp 파일/servlet 파일을 *컴파일러 해주는 역할 (LINK 역할)*
- 결과적으로 html 형식으로 동적인 페이지를 구현 

1. `.jsp`파일의 웹 컨테이너
   - eclipse에서 JSP 파일을 생성한 후 `C:\java\download\apache-tomcat-8.5.37\work\Catalina\localhost\JSP\org\apache\jsp`에 들어가 보면 파일명`_ jsp.java`와 파일명`_jsp.class`파일이 생성된 것을 확인할 수 있음
2. Servlet 파일(java 파일)의 웹 컨테이너
   - 위의 경루로 들어가보면 이 경우에는 `.class`파일이 생성된 것을 확인할 수 있음

<br><br><br>

 [[학습자료 출처\] Inflearn, 신입 프로그래머를 위한 실전 JSP](https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-jsp_renew/)