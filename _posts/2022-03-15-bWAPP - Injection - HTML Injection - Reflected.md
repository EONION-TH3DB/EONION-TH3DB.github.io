---
title: "[bWAPP] 1. Injection - HTML Injection - Reflected - (GET/POST/Current URL)"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## Injection - Reflected

공격자가 태그/스크립트를 삽입한 URL을 클라이언트가 사용했을 경우

서버로 HTTP 요청을 보내고 응답 받을 시 태그/스크립트가 실행되는 방식.

이로 인해 클라이언트는 실제 웹 사이트 접근이 아닌 피싱사이트로 접속하게 되어 개인정보유출 등으로 이어질 수 있음.

<br>

### GET 방식

클라이언트에서 서버로 어떠한 리소스로 부터 정보를 요청하기 위해 사용되는 메서드

어떠한 정보를 가져와서 조회하기 위해 사용되는 방식

- URL에 변수를 포함시켜 요청
- 데이터를 헤더에 포함하여 전송
- URL에 데이터가 노출
- 캐싱 가능

<br>

### POST 방식

클라이언트에서 서버로 리소스를 생성하거나 업데이트하기 위해 데이터를 보낼 때 사용되는 메서드

데이터를 서버로 제출하여 추가 또는 수정하기 위해 사용하는 방식

- URL에 변수 노출하지 않고 요청
- 데이터를 헤더가 아닌 바디에 포함
- URL에 데이터 노출 X
- 캐싱 가능

<br>

<br>

## HTML Injection - Reflected(GET)

![image-20220316001314730](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316001314730.png)

<br>

### Go 버튼 누르기

![image-20220316001409169](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316001409169.png)

- 누른 후 URL을 살펴보면 변수 표시되는 것 확인(GET 방식)

<br>

### 스크립트 삽입

![image-20220316001540137](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316001540137.png)

- lastname에는 아무거나 입력

<br>

### 결과

![image-20220316001718020](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316001718020.png)

<br>

<br>

## HTML Injection - Reflected(GET) - 대응방안

### 리눅스

![image-20220317220850220](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317220850220.png)

![image-20220317221054644](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221054644.png)

- vi 편집기로 htmli_get.php 소스코드를 열어준다.
- 레벨 확인
- medium 에서는 xss_check_1 함수 사용
- high 에서는 xss_check_3 함수 사용

<br>

### functions_external.php

![image-20220317221324822](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221324822.png)

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_1 에서 "<", ">" 문자를 "&lt", "&gt"로 대체하고 있다.
- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.

<br>

<br>

## HTML Injection - Reflected(POST)

![image-20220316001849425](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316001849425.png)

- GET방식과 같은 문제

<br>

### Go 버튼 클릭

![image-20220316002008933](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316002008933.png)

- URL에는 아무런 흔적 없음(POST방식)

<br>

### Burpsuit

![image-20220316002153775](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316002153775.png)

- 화살표 부분이 Body 부분
- 바디 부분의 변수에 스크립트 삽입
- lastname에는 아무거나 입력

<br>

### 결과

![image-20220316002306088](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316002306088.png)

<br>

<br>

## HTML Injection - Reflected(POST) - 대응방안

### 리눅스

![image-20220317221802965](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221802965.png)

![image-20220317221054644](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221054644.png)

- vi 편집기로 htmli_post.php 소스코드를 열어준다.
- 레벨 확인
- medium 에서는 xss_check_1 함수 사용
- high 에서는 xss_check_3 함수 사용

<br>

### functions_external.php

![image-20220317221324822](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221324822.png)

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_1 에서 "<", ">" 문자를 "&lt", "&gt"로 대체하고 있다.
- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.

<br>

<br>

## HTML Injection - Reflected(Current URL)

![image-20220316002548774](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316002548774.png)

<br>

### htmli_current_url.php

![image-20220316002941655](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316002941655.png)

- 비박스 환경에서
- cd /var/www/bWAPP 로 디렉토리 변경 후
- vi htmli_current_url.php 로 vi 편집기를 이용해 현재 문제 php 파일을 열어보게 되면
- your current URL 뒤에 url 변수 확인할 수 있음
- 즉, url이 현 호스트 주소라는 얘기

### BurpSuite

![image-20220316003243104](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316003243104.png)

- 패킷을 잡은 후 Host 주소를 변경해보자

  - 첫번째 방법 : Host를 trust me 라고 변경

  - 두번째 방법 : url 뒤에 스크립트 삽입

    ![image-20220316003710613](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316003710613.png)

<br>

### 결과

첫번째 방법 결과

![image-20220316003549427](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316003549427.png)

- 호스트 주소 변경됨 확인

두번째 방법 결과

![image-20220316003756355](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316003756355.png)

- 현 url 뒤에 쿠키 정보 확인

<br>

<br>

## HTML Injection - Reflected(Current URL) - 대응방안

### Linux

![image-20220317221954769](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221954769.png)

![image-20220317221054644](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221054644.png)

- vi 편집기로 htmli_current_url.php 소스 코드 열람
- medium 일때 document.URL을 출력해주고 있는데
  - php에서 #은 주석으로 처리되지만 뒤에 있는 html 코드는 실행되는 특성을 이용하면 된다.

```html
http://172.30.1.23/bWAPP/htmli_current_url.php#<h1><script>document.write(document.cookie)</script>
```

- high 일때 xss_check_3 함수사용

<br>

### functions_external.php

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.
