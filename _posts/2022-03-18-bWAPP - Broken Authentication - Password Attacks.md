---
title: "[bWAPP] 2. Broken Authentication - Password Attacks"
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

<br>

## Password Attacks - 난이도 : 하

- 패스워드 공격에 관한 문제
- 취약한 패스워드 인증을 사용할 경우 공격당할 수 있는 취약점을 다룬 시나리오

![image-20220318230037655](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318230037655.png)

- Brute Force 기법을 사용해보자

<br>

### BurpSuite

![image-20220318231418886](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318231418886.png)

- 경우의 수가 너무 많기 때문에 시간상 ID는 알고있다 가정하고 PW는 자릿수만 알고있다 가정하겠다.
- 패킷을 잡아줬으면 Intruder로 이동한다.

<br>

![image-20220318231534671](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318231534671.png)

- 공격 타입은 Sniper
- 여러 값에 페이로드가 실려있지만 Clear$로 다 없애주고 Brute Force 를 실행할 변수에만 Add$를 통해 페이로드를 실어주자.

<br>

![image-20220318231658760](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318231658760.png)

- 다음 Payloads 칸으로 이동해서 페이로드 타입을 Brute Forcer 로 바꿔주고
- 길이를 알고있다 가정했으니 3으로 수정해준다.
- 그러고 Start Attack

<br>

 ![image-20220318233510564](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318233510564.png)

- 로그인에 성공했을때와 실패했을때 출력되는 문자열이 다르므로 응답 패킷의 길이로 구분할 수 있다.

<br>

<br>

## Password Attacks - 난이도 : 중

![image-20220318233612886](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318233612886.png)

- 바로 버프수트를 이용해서 패킷을 캡쳐해봤더니 매개변수가 하나 더 추가된 것을 확인할 수 있는데
- Salt라는 함수이다.
- Salt : 평문이 같은 경우 해시 값도 같은 것을 방지하기 위해 평문에 솔트 값을 더한 값을 해시 함으로써 같은 평문이더라도 다른 해시 값으로 패스워드 크래킹을 어렵게 하는 함수이다.
- 이 Salt가 어떻게 쓰였는지 웹페이지 소스코드로 들어가보자.

<br>

### ba_pwd_attacks_2.php

![image-20220318233904243](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318233904243.png)

- 166~167 줄에 Salt에다가 랜덤한 스트링을 부여하고 세션함수로 인해 로그인 시도시마다, salt 값이 서버에서 계속 랜덤한 값으로 바뀐다.
- 그리고 138 줄에 패스워드를 확인할 때 사용자가 가지고 있는 salt 값과 세션 변수에 저장된 salt 값을 비교하고 있다.
- 즉, Medium 레벨에서는 Brute Force 공격 시 한 번 대입할 떄마다 salt 값이 변하기 때문에 Brute Force 공격은 통하지 않는다.

<br>

<br>

## Password Attacks - 난이도 : 상

![image-20220318234257737](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318234257737.png)

- 난이도 상에서는 캡챠 방식의 로그인을 이용하고 있는데
- 이 캡챠를 reload하고나 로그인 버튼을 직접 클릭하지 않는 이상 capcha 값만 고정시켜 Brute Force 공격을 시행할 수 있다.
