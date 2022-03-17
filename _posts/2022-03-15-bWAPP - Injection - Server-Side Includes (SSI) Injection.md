---
title: "[bWAPP] 1. Injection - Server-Side Includes (SSI) Injection"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## Server-Side Includes (SSI) Injection

HTML 페이지에 사용하는 지시어로, 페이지를 서비스할때 서버가 처리

SSI를 사용하면 CGI 프로그램이나 다른 동적인 기술로 페이지 전체를 만들어서 서비스하지 않고도 HTML 페이지에 동적으로 생성한 내용을 추가 가능

보통 HTML 문서는 웹 서버에서 아무런 처리 없이 클라이언트 측으로 전송하지만,

SSI 문서에 대해서는 클라이언트 측으로 보내주기 전에 웹서버가 HTML 문서 안에 있는 내용 중에 웹 서버가 인식하는 특별한 명령어들을 처리(Pasing)한 후에 전송

![image-20220316024702447](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316024702447.png)

<br>

### 입력

![image-20220316024740667](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316024740667.png)

![image-20220316030325422](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316030325422.png)

- 이런식으로 보여주는 갑다.
- 현 페이지인 ssii.shtml의 소스를 살펴보자

<br>

### ssii.html

![image-20220316030000271](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316030000271.png)

![image-20220316030151116](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316030151116.png)

- Your IP address 뒤에 오는 구문 확인
- `<!-- exec -->` 코드를 이용하자

<br>

### 스크립트 삽입

![image-20220316030536441](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316030536441.png)

- First name : <!--#exec cmd ="id"-->
- Last name : <!--#exec cmd ="cat /etc/passwd"-->

<br>

### 결과

![image-20220316030809473](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316030809473.png)

- 성공적으로 개인정보 출력

<br>

<br>

## 대응방안

### Linux

![image-20220317231242158](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317231242158.png)

![image-20220317221054644](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221054644.png)

- vi 편집기로 ssii.php 소스코드 열어준다.
- 레벨확인
- Medium : xss_check_4 함수사용
- High : xss_check_3 함수사용

<br>

### functions_external.php

![image-20220317224046908](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317224046908.png)

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_4 에서 addslashes 함수 사용
- addslashes 는 특수 문자(',",/)앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수 
  - 하지만 인코딩된 태그나 스크립트가 들어오게 되면 바로 디코딩되기 때문에 보안이 뚫린다.
- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.
