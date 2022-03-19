---
title: "[bWAPP] 2. Session Management - Administrative Portals"
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

## Administrative Portals - 난이도 : 하

세션 관리중 관리자 페이지 접근에 관한 취약점을 다루는 문제

![image-20220319191609972](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191609972.png)

- 힌트에 URL을 체크하라고 한다.

![image-20220319191701034](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191701034.png)

- 매개변수 발견
- 값을 1로 바꿔서 바꿔서 보안을 뚫어보자.

<br>

![image-20220319191758559](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191758559.png)

![image-20220319191813149](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191813149.png)

- 난이도 하에서는 간단하게 URL에 노출된 매개변수를 이용해 보았다.

<br>

<br>

## Administrative Portals - 난이도 : 중

![image-20220319191923636](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191923636.png)

![image-20220319191931477](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319191931477.png)

- 난이도 중에서는 URL에 노출되는 매개변수는 보이지 않고
- 힌트를 보니 쿠키를 체크하라고 한다.

<BR>

### 쿠키 체크

![image-20220319192016425](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319192016425.png)

- 쿠키를 체크했더니 admin 값과 세션ID, Security_level이 있는 걸 확인할 수 있고
- 관리자 계정으로 접근하는 문제이다 보니 admin 값을 1로 바꿔주겠다.

![image-20220319192148940](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319192148940.png)

- 쿠키값을 바꿔주고 새로고침을 눌러줬더니 성공

<br>

<br>

## Administrative Portals - 난이도 : 상

![image-20220319192233012](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319192233012.png)

- 난이도 상에서는 관리자 계정이 아닌 테스트 계정으로 로그인하게 되면 DB 관리자의 도움으로 페이지를 언락하라고 한다.
- 쿠키와 URL을 확인해보면 당연히 노출되는 변수가 보이지 않는다.
- 적절하게 보안이 적용되어있는 걸로 판단하여 바로 레벨 별 대응방안으로 넘어가겠다.

<BR>

<BR>

## 대응방안

### Linux

![image-20220319193257820](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319193257820.png)

- 난이도 하일 때 GET 방식 서버 요청으로 admin 값이 1 일때 성공 문구 출력하고

<br>

![image-20220319193423002](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319193423002.png)

- 난이도 중일 때 COOKIE 함수를 사용해 쿠키 admin 값이 1인지 확인하여 성공문구를 출력하고 있다.

<br>

![image-20220319193518910](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319193518910.png)

- 난이도 상에서는 세션함수를 이용해 세션 배열의 admin 값이 1인지 체크하여 관리자 페이지 접속을 허용하고 있다.
- 이런식으로 관리자가 아닌 일반 사용자에 대한 관리자 페이지 접근을 제한할 수 있다.