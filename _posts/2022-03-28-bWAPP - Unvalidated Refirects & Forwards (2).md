---
title: "[bWAPP] 10. Unvalidated Redirects & Forwards (2)"
---

<br>

## Unvalidated Redirects & Forwards

### Redirection

웹 애플리케이션에 접속한 사용자를 다른 페이지로 이동하게 하는 역할.

리다이렉션 시 목적지에 대한 검증이 이루어지지 않으면 공격에 사용될 수 있는 취약점이 되고, 특히 악성 사이트나 피싱 등의 공격으로 사용자에게 피해가 발생한다.

또한, 관리자 단말 PC에 악성코드가 감염되면 순식간에 내부 시스템으로 침투할 수 있다.

<br><br>

## Unvalidated Redirects & Forwards (2)

### 난이도 : 하

![image-20220328172218189](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328172218189.png)

- here 버튼을 클릭하면 BeeBox 메인 페이지로 이동하는 시나리오이다.
- 메인 페이지로 이동하는 패킷을 버프수트로 가로채서 리다이렉션 취약점을 다뤄보자.

<br>

### BurpSuite

![image-20220328172534055](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328172534055.png)

- 특정 페이지로 이동하는 변수 ReturnUrl 발견.
- portal.php 를 지우고 원하는 사이트를 입력해보자.

<br>

![image-20220328172643756](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328172643756.png)

- 입력 후 intercept is off 로 바꿔주면

<br>

![image-20220328172715144](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328172715144.png)

- 다음과 같이 성공적으로 이동한 리다이렉션 취약점을 볼 수 있다.

<br>

### <br>

## 대응방안

### Linux

![image-20220328172910681](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328172910681.png)

- vi 편집기로 unvalidated_redir_fwd_2.php 소스코드를 열어주자.
- 먼저 '난이도 : 하'일 때 아무런 보안조취가 되어있지 않아 요청받는 ReturnUrl 변수로 반환되는 값으로 header 함수를 통해 이동하고 있다.
- 그리고 '난이도 : 중, 상'일 때를 보면 header 함수 인자에  'portal.php'로 값이 고정되어 있는 것을 확인할 수 있다.
- header 값을 정적으로 정의하므로 ReturnUrl 변수를 조작한 리다이렉션 요청은 불가능하도록 보안설정이 이루어져있는 것을 확인할 수 있다.