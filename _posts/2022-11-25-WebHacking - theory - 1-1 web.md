---
title: "[Web Hacking] 이론 - 1-1 웹"
---

<br>

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125192248572.png" alt="image-20221125192248572" style="zoom: 67%;" />

# Web Hacking

1. 이론
   1. **웹** &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;☚
      1. **정의**
      2. **웹 브라우저**
      3. **웹 애플리케이션**
      4. **DBMS**
      5. **HTTP**
      6. **웹 서버 / 웹 애플리케이션 서버**
      7. **Cookie / Session**
   2. JAVA/JSP
   3. JSON
2. 실습
   1. 2013 OWASP TOP 10 - bWAPP
3. 심화
   1. Injection
   2. File Upload / WebShell
   3. File Download
   4. XSS

<BR><br>

# 웹

## 정의

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125193507204.png" alt="image-20221125193507204" style="zoom:80%;" />

**월드 와이드 웹**(World Wide Web, WWW, W3)을 간단하게 줄여 웹이라고 한다.

영어로 사전적 의미 '**거미줄**'이라는 뜻.

인터넷상에서 **각각의 사용자가 연결**되어 서로 정보를 **공유**한다는 의미에서 유래되었다.

즉, 웹이란 인터넷상의 서비스 중 **HTTP**를 이용하여 정보를 **공유**하는 **통신 서비스**를 말한다.

<br>

## 웹 브라우저

![image-20221125193745855](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125193745855.png)

색을 칠하기 위해서는 **색연필**이 필요하듯이 웹을 사용하기 위해서는 **웹 브라우저**가 필요하다.

웹 서버에서 이동하며 **쌍방향**으로 통신하고 HTML 문서나 파일을 **출력**하는 **그래픽 사용자 인터페이스** 기반의 응용 소프트웨어이다.

<br>

## 웹 애플리케이션

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125195559213.png" alt="image-20221125195559213" style="zoom:80%;" />

사용자의 요청을 **동적**으로 처리할 수 있도록 만들어진 프로그램

<br>

## DBMS

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125195757998.png" alt="image-20221125195757998" style="zoom:80%;" />

데이터를 저장하기 위해 사용하는 데이터 저장소관리 프로그램

DB 내의 데이터 조회, 수정, 삽입을 용이하게 할 수 있도록 도와주는 서버 애플리케이션이다.

대표적으로, MySQL, MS-SQL이 있고 사용자의 개인정보가 포함되어있기 때문에 보안이 중요하다.

<br>

## HTTP

![image-20221125194732347](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125194732347.png)

Web 상에서 정보를 주고받을 수 있는 프로토콜로써, 주로 HTML 문서를 주고 받는데에 쓰인다.

클라이언트와 서버 사이에 이루어지는 요청 / 응답 프로토콜로써,

클라이언트인 웹 브라우저가 HTTP를 통해 서버로부터 웹 페이지(HTML)나 그림 정보를 요청하면,

서버는 이 요청에 응답하여 필요한 정보를 해당 사용자에게 전달한다.

<br>

## 웹 서버 / 웹 애플리케이션 서버

![image-20221125194129559](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125194129559.png)

### 웹 서버

HTTP를 통해 웹 브라우저에서 요청하는 HTML 문서나 오브젝트(이미지, 파일 등)을 전송해주는 서비스 프로그램이다.

- HTTP 프로토콜을 기반으로 클라이언트의 HTTP 요청을 서비스 하는 기능

- 정적인 콘텐츠 제공

  ​	(WAS를 거치지 않고 바로 HTTP 응답)

- 동적인 콘텐츠 제공

  ​	웹 서버가 동적인 콘텐츠를 제공하기 위해 동적인 콘텐츠 요청을 WAS에 전달하고 WAS가 클라이언트에게 응답

- Apache, IIS, Nginx

### 웹 애플리케이션 서버(WAS)

 DB 조회나 다양한 로직 처리를 요구하는 동적인 콘텐츠를 제공하기 위해 만들어진 애플리케이션 서버.

웹 서버 안에 웹 애플리케이션 서버가 있다고 보면 된다.

- 웹서버 + 웹 컨테이너
- 동적인 콘텐츠 제공
- Tomcat, JBoss, Jeus

<br>

## Cookie / Session

![image-20221125201233096](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125201233096.png)

쿠키는 HTTP의 Connectionless, Stateless 특성을 보완하기 위해 등장했다.

Connectionless

- 서비스 연결상태 유지는 서버 부하를 일으키기에 웹 특성상 연결비용을 줄이는 특성이다.

Stateless

- 통신이 끝나면 상태를 유지하지 않는 특성으로써, 연결을 끊는 순간 클라이언트, 서버의 통신 종료 및 상태 정보를 유지하지 않는 특성이다.

### 쿠키

클라이언트 로컬에 저장되는 키와 값이 들어있는 작은 파일

- 사용자 인증이 유효한 시간 명시하는데, 유효시간이 정해지면 브라우저를 종료해도 인증이 유지된다.
- 데이터에 인증 상태를 포함하기 때문에 사용자가 임의의 사용자로 인증된 것처럼 요청이 조작될 우려가 있다.
- 또한, 쿠키는 클라이언트 로컬에 저장되어서 요청 시 변질되거나 스니핑 당할 우려가 있기에 세션이 등장하게 된다.

원리

1. 클라이언트 요청
2. 서버에서 쿠키 생성
3. HTTP 헤더에 쿠키 포함시겨 응답
4. 브라우저 종료되어도 쿠키가 유효하다면 클라이언트 로컬에 보관
5. 같은 요청 시 HTTP 헤더에 보관한 쿠키를 함께 요청
6. 서버에서 쿠키 정보 확인(변경 필요 시 쿠키 업데이트하여 변경된 쿠키를 HTTP 헤더에 넣어 응답)

### 세션

쿠키의 단점들을 보완하기 위해 등장한게 세션이다.

- 쿠키를 기반으로 하고있지만, 사용자 데이터를 브라우저(로컬)에 저장하는 쿠키와는 달리 세션은 서버에서 저장하고 관리한다.
- 또한, 서버 측에서는 클라이언트를 구분하기 위해 세션 ID를 부여하며, 서버에 접속해 종료할 때까지만 세션이 유지되는 특징이있다.

원리

1. 클라이언트가 서버에 접속 시 세션 ID 발급
2. 클라이언트는 세션 ID에 쿠키 저장
3. 클라이언트가 서버에 요청 시, 쿠키+세션ID를 서버에 같이 요청
4. 서버는 세션 ID를 전달받아 별다른 작업없이 세션 ID로 세션에 있는 클라이언트 정보 가져와서 처리 후 응답

### 차이점

메모리

- 사용자 수만큼 서버 메모리 차지하기에 토큰 기반의 인증 처리 방식이 상요되는 추세이다.

속도

- 처리할게 적은 쿠키가 더 빠르다.

만료시간(라이프 싸이클)
