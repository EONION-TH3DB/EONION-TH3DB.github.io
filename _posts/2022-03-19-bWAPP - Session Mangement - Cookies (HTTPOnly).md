---
title: "[bWAPP] 2. Session Management - Cookies (HTTPOnly)"
---

<br>

## Session Mangement

사용자가 서버에 ID/PW 등을 통해 인증된 경우 접속 유지 역할을 수행하는 세션이 생성되는데 이러한 세션은 사용자 인증, 접속 관리, 권한 관리 등으로 사용된다.

이러한 세션이 취약하게 관리되거나 사용자 세션에 대한 적정성 검토가 제대로 이루어지지 않을 경우 공격자에 의해 권한 상승, 계정 탈취 등의 위험도 높은 공격으로 이루어질 수 있다.

<br>

### Session Hijacking

공격자가 인증 작업 등이 완료되어 정상통신을 하고 있는 다른 사용자의 세션을 가로채 별도의 인증 작업 없이 가로챈 세션으로 통신을 계속하는 행위

ID, PW를 몰라도 시스템에 접근가능한 공격이며,

인증 작업이 완료된 세션을 공격하기 때문에 OTP, Challenge/Response 기법을 사용하는 사용자 인증을 무력화 시킴.

<br>

<br>

## Cookies (HTTPOnly) - 난이도 : 하

쿠키에는 세션 ID가 저장되는데, 쿠키의 옵션 중 HttpOnly에 관한 문제이다.

![image-20220319193828971](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319193828971.png)

- HTTPOnly는 Set-Cookie 라는 php 함수의 옵션인데
  - Set-cookie : 서버가 클라이언트의 쿠키를 설정하는 함수
- XSS 와 같은 공격으로부터 쿠키 탈취를 방지하기 위해, JavaScript의 document.cookie 함수를 사용해 쿠키에 접근하는 것을 못하게 하는 옵션이다.

- 사진에 나와있는 Cookies 버튼과 here 파워링크를 클릭해보고 추가적으로 개발자모드 콘솔에서 document.cookie를 이용해 쿠키값을 출력해보자.

<br>

![image-20220319194258826](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319194258826.png)

![image-20220319194326086](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319194326086.png)

![image-20220319194539492](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319194539492.png)

- 다음과 같이 모든 쿠키값이 출력되는 것을 확인할 수 있다.
- 아마 난이도 하여서 HttpOnly 옵션이 설정되어 있지 않은 것 같다.
- 그렇다면 버프수트를 이용해 set-cookie 값을 들여다보자

<br>

### BurpSuite

![image-20220319194748294](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319194748294.png)

- 패킷을 Intercept 하기전에 요청값을 꺼주고 응답값을 켜준 후 응답 패킷을 살펴보겠다.
- set-cookie 함수는 응답 패킷에서 확인할 수 있기 때문이다.

<br>

![image-20220319194910418](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319194910418.png)

- 확인했을 떄 top_security에 httponly 옵션을 적용시켜 레벨 별 확인을 하는 시나리오인 것 같다.
- 아무튼 set-cookie 확인 시 설정되어있지 않은 것을 확인했기에 top_security 값이 레벨 하에서는 노출되는 것을 확인할 수 있다.

<br>

<br>

## Cookies (HTTPOnly) - 난이도 : 중

![image-20220319195136381](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195136381.png)

- 난이도 중이다. 똑같이 Cookies와 here, 개발자모드 콘솔에서 document.cookie를 입력해보겠다.

<br>

![image-20220319195237435](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195237435.png)

![image-20220319195255226](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195255226.png)

- HttpOnly 옵션의 활성화로 인해 웹페이지에는 출력되지만, document.cookie를 활용한 자바스크립트나 콘솔에서는 출력되지 않은 것을 확인할 수 있다.

<br>

### BurpSuite

![image-20220319195433697](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195433697.png)

- HttpOnly 옵션이 설정되어 있는 것을 확인할 수 있다.

<br>

<br>

## Cookies (HTTPOnly) - 난이도 : 상

![image-20220319195533948](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195533948.png)

![image-20220319195602649](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195602649.png)

- 난이도 상 또한 중과 마찬가지로 HttpOnly 옵션이 활성화되어 있는 걸로 판단 된다.

<br>

### BurpSuite

![image-20220319195713252](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195713252.png)

- 마찬가지로 httponly 옵션 활성화되어 있다.
- 그렇다면 난이도 중과 상의 차이는 무엇이 있을지 대응방안에서 살펴보겠다.

<br>

<br>

## 대응방안

### Linux

![image-20220319195838495](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319195838495.png)

- vi 편집기로 smgmt_cookies_httponly.php 소스코드를 열어주자.
- setcookie("쿠키 이름", "쿠키 값", 유효기간, 경로, 호스트명, secure, httponly)
  - 즉, 난이도 0(하)에서 httponly를 false 비활성화하고 있고
  - 난이도 1(중)에서는 httponly true를 통해 활성화하고 있으며
  - 난이도 2(상)에서도 마찬가지로 httponly 옵션을 활성화하여 document.cookie에 대한 접근을 방지하고 있다.
- 그럼 우리는 이제 중과 상의 차이점을 알아보겠다
  - 눈치 챘다싶이 유효기간에서의 차이로 top_security의 maybe와 yes의 여부를 나누고 있다.
  - 유효기간을 한시간에서 5분으로 줄여 타인에 대한 접근을 막고 있다.