---
title: "[Dreamhack] War Game 1단계 - Web XSS-1"
---

<br>

## 문제

![image-20220106235712466](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220106235712466.png)

- XSS(Cross Site Scripting)

  사이트 간 스크립팅

  웹 애플리케이션에서 많이 나타나는 취약점

  웹 사이트 관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점

- 주로 여러 사용자가 보게되는 전자 게시판에 악성 스크립트가 담긴 글을 올리는 형태

  웹 애플리케이션이 사용자로부터 입력받은 값을 제대로 검사하지 않고 사용할 경우 발생

- 해커가 사용자의 정보인 쿠키나 세션 등을 탈취하거나 자동으로 비정상적인 기능 수행 가능

  주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 함

<br><br>

## 접속 정보

![image-20220107001509377](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107001509377.png)

<br>

### vuln(xss) page

![image-20220107004957255](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107004957255.png)

- param 매개변수로 입력된  값이 전송되어, 페이지 응답 시 실행된 모습
  - 이를 통해 스크립트 공격이 가능할꺼라 판단

<br>

### memo

![image-20220107004929236](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107004929236.png)

- memo 매개변수로 입력된 값이 전송되어, 페이지 응답 시 화면에 출력

<br>

### flag

![image-20220107005051330](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107005051330.png)

![image-20220107002602571](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107002602571.png)

- 아무 값을 입력하고 제출을 눌러보면 good이라는 메시지가 보임.
  - 스크립트를 넣어 공격할 페이지인 것 같다.

<br><br>

## 소스 코드

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107001404485.png" alt="image-20220107001404485" style="zoom:150%;" />

app.route를 통해 ''접속정보''의 각 페이지들의 특성을 알아보자

- vuln 함수

  - vuln 페이지에 문자열 필터링이 없기 떄문애 param 매개변수에 스크립트 삽입 시 스크립트 동작

- memo 함수

  - memo 파라미터에 입력된 내용을 화면에 출력

- flag 함수

  - check_xss 함수로 flag라는 이름의 FLAG.strip() 값 념거줌
    - 플래그 값
  - check_xss 함수
    - 정의한 url과 쿠키(flag이름의 FLAG.strip())값을 read_url 함수로 넘겨줌
  - read_url 함수
    - cookie.update 명령어로 127.0.0.1을 도메인으로 지정하고
    - driver.get("http://127.0.0.1:8000/")을 통해 로컬호스트 환경에서 웹 페이지에 접속
    - driver.add_cookie(cookie) : 쿠키를 추가하고
    - driver.get(url = f"http://127.0.0.1:8000/vuln?param={urllib.parse.quote(param)}")로 다시 접속

- 즉, flag 함수는

  - flag 페이지에서 사용자가 입력한 값을 param 매개변수를 통해 전송하면
  - 로컬호스트 환경에서 웹페이지에 접속 후 FLAG 값을 쿠키값으로 추가하고
  - 사용자가 입력한 param 매개변수 값에 접속한다.

- 접근법

  - flag 페이지의 param 매개변수 값을 조작하여 
  - 로컬호스트 환경에서 웹페이지에 접속 후 FLAG 값인 쿠키값을 추가하면
  - 해당하는 쿠키 값을 memo 매개변수로 전달해 memo 페이지에서 FLAG 값이 출력되게 하자.

- 스크립트

  `<script>location.href="http://127.0.0.1:8000/memo?memo=" + document.cookie;</script>`

  - location.href
    - href 는 location 객체에 속해있는 프로퍼티로 현재 접속중인 페이지 정보 가짐
    - 또한, 값을 변경하는 프로퍼티이기 때문에 다른 페이지로의 접속도 가능
  - document.cookie
    - 쿠키 출력

<br><br>

## 결과

![image-20220107005227435](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220107005227435.png)

```
flag=DH{2c01577e9542ec24d68ba0ffb846508e}
```