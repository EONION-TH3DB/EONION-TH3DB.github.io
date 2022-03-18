---
title: "[bWAPP] 1. Broken Authentication - Logout Management"
---

<br>

## Broken Authentication

인증과 세션 관리와 관련된 애플리케이션 기능이 정확하게 구현되어 있지 않아서

공격자가 패스워드, 키 또는 세션 토큰을 위험에 노출 시킬 수 있거나 

일시적 또는 영구적으로 다른 사용자의 권한 획득을 위해 구현상 결함을 악용하도록 허용한 것을 말함.



### Brute - Force Access

무차별 대입 공격

조합 가능한 모든 경우의 수를 다 대입하여 올바른 암호가 발견될 때까지 시도하는 과정

<br>

### Credential Stuffing

  <img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/EMB00002df83ccf.JPG" alt="img" style="zoom: 25%;" />

  수집된 사용자 이름과 비밀번호를 자동으로 대입하여 사용자 계정에 부전하게 엑세스하려는 공격.

일종의 Brute Force 기법이지만

데이터 침해에서 입수한 알려진 유효 인증 정보 목록을 이용한다는 점에서 차이가 있음.

<br>

### Dictionary

![image-20220318165055489](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318165055489.png)

무차별 대입이라는 특징을 그대로 구현한 '무작위 순차 대입'과

상당한 시간이 소요되는 무차별 대입 방식의 단점을 극복 및 보완한 공격 기법.

미리 정의된(사전 파일) 문자열 목록을 대입하는 방법.

사전 파일은 그동안 통계적으로 많이 사용되거나, 사람들이 흔히 사용할법한 비밀번호 조합을 미리 목록으로 정의해 둔 파일.

<br>

### Session Hijacking

공격자가 인증 작업 등이 완료되어 정상통신을 하고 있는 다른 사용자의 세션을 가로채 별도의 인증 작업 없이 가로챈 세션으로 통신을 계속하는 행위

ID, PW를 몰라도 시스템에 접근가능한 공격이며,

인증 작업이 완료된 세션을 공격하기 때문에 OTP, Challenge/Response 기법을 사용하는 사용자 인증을 무력화 시킴.

<br>

<br>

## Logout Management

![image-20220318224728859](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224728859.png)

- here 클릭

![image-20220318224754948](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224754948.png)

- 뒤로가기

![image-20220318224728859](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224728859.png)

- 뒤로가기를 누르니 다시 로그인 되었던 페이지로 돌아온 것을 확인할 수 있는데 코드를 보며 설명을 하겠다.

<br>

### ba_logout_1.php

![image-20220318224932992](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224932992.png)

- vi 편집기로 ba_logout_1.php를 열어주면 되는데
- here로 이동할 때 나타나는 페이지의 소스코드이다.
- here에 마우스를 올려놓으면 왼쪽하단에 url이 뜬다.

<br>

![image-20220318225313051](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318225313051.png)

- 공통적으로 로그아웃시 쿠키를 삭제해주고 있는데
- case 0 난이도 하 에서는 세션에 대해 아무런 조치를 취하고 있지 않기 때문에 로그아웃시 세션이 살아있어 다시 그 로그인 상태의 페이지로 갈 수 있었던 것이다.
- case 1 난이도 중을 보면 session_destroy를 통해 현제 세션과 관련된 모든 데이터를 삭제해주고있어서 뒤로가기를 눌러도 로그아웃 페이지 그대로 일 것이다.
- case 2 난이도 상은 session_destroy를 해주기 전에 $_SESSEION = array(); 를 수행해주고있는데 
- $_SESSEION = array();는 세션 변수를 비우는 코드로 session_unset() 함수의 기능과 같다.