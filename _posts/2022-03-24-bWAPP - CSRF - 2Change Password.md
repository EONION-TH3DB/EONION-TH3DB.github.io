---
title: "[bWAPP] 8. Cross-Site Request Forgery - Change Password"
---

<br>

## Cross-Site Request Forgery

크로스 사이트 요청 변조는 공격자에 의해 사용자가 의도하지 않은 악의적인 행위를 서버에 요청하는 취약점.

공격자가 입력한 악의적인 스크립트 코드에 접근하면 사용자의 웹 브라우저는 스크립트 코드에서 의도한 내용을 웹 서버에 요청한다.

웹 서버는 변조된 요청을 사용자의 정상적인 요청으로 판단한여 응답을 보낸다.

<br><br>

## CSRF와  XSS의 차이

CSRF는 웹 브라우저를 신뢰해서 발생하는 취약점으로, 웹 사이트 사용자가 보낸 정상적인 요청과 변조된 요청을 구별하지 못한다.

또한, XSS는 공격 대상이 사용자이지만, CSRF는 사용자가 공격의 매개물로 공격자를 대신하여 악의적인 요청을 웹사이트에 요청한다.

<br><br>

## Change Password

![image-20220325004324404](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325004324404.png)

- 접속한 사용자의 비밀번호를 변경할 수 있는 시나리오이다.

<br>

![image-20220325004359940](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325004359940.png)

- Change 버튼을 눌러보면 GET 메서드로 HTTP 연결 요청을 하므로 URL에 변수를 노출한다.
- 그렇다면 다음과 같은 코드를 삽입하자.
- 넓이와 높이가 0인 이미지 태그에 사용자 비밀번호를 변경하는 페이지를 호출하여 비밀번호를 eonion으로 변경해보자.

```html
<img src= "http://172.30.1.16/bWAPP/csrf_1.php?password_new=eonion&password_
conf=eonion&action= change" height= "0" width= "0">
```

<br>

### htmli_stored.php

![image-20220325004914919](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325004914919.png)

- 다음과 같이 Entry에는 높이와 넓이가 0이기 때문에 아무것도 보이지 않고 코드만 실행됐을 것이다.

<br>

### sqli_16.php

![image-20220325004955637](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325004955637.png)

- 다음과 같이 bug가 아닌 eonion으로 로그인 시 성공적으로 로그인 되는 것을 알 수 있다.

<br>

<br>

## 대응방안

### Linux

![image-20220325005326554](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325005326554.png)

- vi 편집기로 csrf_1.php 소스코드를 열어주자
- 현 시나리오에서는 난이도와 상관없이 새로운 비밀번호를 입력하는 password_new 변수에 mysql_real_escape_string을 사용하여 우회한다.
- 따라서 SQL 인젝션 등 다른공격은 되지 않고 이미지 태그를 이용한 삽입 공격만이 가능하다.

<BR>

![image-20220325005521378](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325005521378.png)

- 다음으로 난이도 중, 상에서는 password_curr 변수를 추가해 사용자의 현재 비밀번호도 입력을해야 새 비밀번호로 변경된다.
- password_curr 변수 또한  mysqli_real_escape_string 함수로 입력값을 우회한다.
- mysqli_real_escape_string
  - SQL 문법에서 사용되는 특수 문자들에 백슬래시를 붙여 입력 값을 SQL 문법으로 인식되지 않게 방어하는데, 백슬래시 가 붙는 문자는 NULL, \n, \r, \, ', ", ^Z이다.
- 또한, 소스코드에서 SHA-1 해시 함수를 이용해 password_curr 변수의 입력 값을 해시 값으로 변환한 후 DB에 저장된 비밀번호 해시 값과 비교한다.
- 따라서 비밀변호를 변경하기 위해서는 현재 비밀번호가 필요함을 알 수 있다.

<br>

<br>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습