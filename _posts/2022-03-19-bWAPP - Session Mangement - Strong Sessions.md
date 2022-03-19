---
title: "[bWAPP] 2. Session Management - Strong Sessions"
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

## Strong Sessions - 난이도 : 하

세션 하이재킹을 이용해서 관리자 계정으로 접근하는 시나리오

![image-20220319210737792](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210737792.png)

- Cookies를 눌러보자

![image-20220319210819025](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210819025.png)

- 쿠키 정보가 출력.
- here를 클릭해보자

![image-20220319210948742](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210948742.png)

- 특정 페이지로 이동한다. 
- 시크릿 모드에서 관리자 계정으로 접속하여 세션아이디 쿠키값을 탈취한 후에
- test11 계정의 쿠키값에 붙여넣어 준 후 관리자 계정으로 접속해보자.

<br>

![image-20220319211137842](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319211137842.png)

- 관리자 계정의 쿠키값
- 복사

![image-20220319211205758](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319211205758.png)

- test 계정의 쿠키값
- 붙여 넣기 해준다.

<br>

![image-20220319210844615](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319210844615.png)

- 계정 바뀐 것 확인

![image-20220319211311297](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319211311297.png)

- home 으로 이동해서 다음과 같이 관리자 계정의 패스워드를 변경할 수 있다.

## 대응방안

### Linux

![image-20220319212549212](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319212549212.png)

- vi 편집기를 이용해 smgmt_strong_sessions.php 소스코드를 열어주자
- case 0 (하) 에서 top_security_ssl 과 top_security_nossl 쿠키를 삭제해줌으로써 출력되지 않는다.

<br>

![image-20220319212757090](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319212757090.png)

- case 1 (중) 에서 top_security_ssl을 삭제해주고 random한 값의 토큰을 해시함수화하여 top_security_nossl의 값으로 선언하고 있는데 httponly만 설정되어 있고 secure 함수는 비활성화 되어있다.

<br>

![image-20220319212927966](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220319212927966.png)

- case 2 (상) 에서는 top_security_nossl을 삭제해주고 random한 토큰을 해쉬함수화하여 top_securty_ssl의 값으로 넣어주고 있는데 secure 옵션과 httponly 옵션 모두 활성화되어 있다.
  - top_security_ssl 해쉬를 구하고자 한다면 https로 통신해야 볼 수 있다.
- 즉, 이 문제에서 나타내고자 하는 바는 
  - 레벨 별로 쿠키값을 보여줌으로써 설정할수 있는 쿠키값들의 옵션을 통해 강한 보안이 적용된 세션의 정도를 보여주는 것 같다.