---
title: "[bWAPP] 10. Unvalidated Redirects & Forwards (1)"
---

<br>

## Unvalidated Redirects & Forwards

### Redirection

웹 애플리케이션에 접속한 사용자를 다른 페이지로 이동하게 하는 역할.

리다이렉션 시 목적지에 대한 검증이 이루어지지 않으면 공격에 사용될 수 있는 취약점이 되고, 특히 악성 사이트나 피싱 등의 공격으로 사용자에게 피해가 발생한다.

또한, 관리자 단말 PC에 악성코드가 감염되면 순식간에 내부 시스템으로 침투할 수 있다.

<br><br>

## Unvalidated Redirects & Forwards (1)

### 난이도 : 하

![image-20220328170604116](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328170604116.png)

- 드롭다운 메뉴에 있는 사이트를 선택하고 [Beam] 버튼을 클릭시 해당 사이트로 이동하는 시나리오이다.
- 버프수트로 [Beam]을 눌렀을 때 이동하는 패킷을 캡쳐해보자.

<br>

### BurpSuite

![image-20220328170803465](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328170803465.png)

- 다음과 같이 url 변수에 이동하는 사이트를 입력받는다.
- 원하는 url 주소를 입력해보자

<br>

![image-20220328170919581](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328170919581.png)

- intercept is off로 바꿔주면

<br>

![image-20220328170954242](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328170954242.png)

- 다음과 같이 리다이렉션 취약점을 활용할 수 있다.

<br>
<br>

## 대응방안

### Linux

![image-20220328171100598](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328171100598.png)

- vi 편집기를 이용해 unvalidated_redir_fwd_1.php 소스코드를 열어주자.
- 먼저 Level Low 단계에서 아무런 보안 조치가 되어있지 않기 때문에 url 요청이 들어오면 header 함수로 등록되어 요청된 url 주소로 이동하게 된다.

<br>

### 난이도 : 중,상

![image-20220328171313593](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328171313593.png)

![image-20220328171334167](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328171334167.png)

![image-20220328171401066](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220328171401066.png)

- 난이도 중,상일 때 패킷을 살펴보면 공통적으로 url에 숫자가 들어가는 것을 확인할 수 있다.
- url 변수에 숫자만 받을 수 있게 설정하여 case 함수로 해당 지정 숫자로 이동하는 리다이렉션을 확인할 수 있고
- 또한, 난이도 상에서 웹 사이트를 선택하고 연결을 요청하면 'session_destroy' 함수로 현재 사용중인 세션의 데이터를 삭제한다.
- 'session_destroy' 함수는 쿠키에 포함된 세션 정보는 삭제하지 않기 때문에 추가적으로 set_cookie 함수를 통해 쿠키에 포함된 정보까지도 삭제해주고 있다.