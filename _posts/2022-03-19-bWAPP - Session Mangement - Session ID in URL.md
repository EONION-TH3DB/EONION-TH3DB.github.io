---
title: "[bWAPP] 2. Session Management - Session ID in URL"
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

## Session ID in URL - 난이도 : 하

유출되는 세션을 하이재킹하여 계정에 접근하는 시나리오

![image-20220319205129395](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319205129395.png)

- 타 계정에서 접근하기 위해 Ctrl + Shift + N 의 크롬 시크릿 모드로 접근해서 test11 계정으로 접속하자

![image-20220319205357111](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319205357111.png)

- test11 계정으로 로그인하고 URL을 살펴보면 GET 방식으로 통신하고 있기에 URL에 세션 ID 값이 나오는 것을 확인할 수 있다.
- bee 계정의 세션 ID를 가로채서 시크릿모드 test11의 URL에 붙여넣기 해주자.

<BR>

![image-20220319205922127](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319205922127.png)

- 계정이 test11에서 Bee로 바뀌는 것을 확인할 수 있다.

<br>

<br>

## Session ID in URL - 난이도 : 중, 상

![image-20220319210057560](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210057560.png)

- URL에 매개변수가 출력되지 않고 있다.

<br>

<br>

## 대응방안

![image-20220319210222472](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210222472.png)

- Level 0 (하)에서 GET 방식으로 세션아이디 쿠키값을 넘겨 주기 때문에 URL에 쿠키값이 노출되는 현상이 있었다.
- Level 1, 2 (중, 상)에서는 GET 방식으로 통신하여 URL에 매개변수를 보여주는 것보다는 안전하다.