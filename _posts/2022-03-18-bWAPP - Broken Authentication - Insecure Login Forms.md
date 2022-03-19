---
title: "[bWAPP] 2. Broken Authentication - Insecure Login Forms"
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

## Insecure Login Forms - 난이도 : 하

로그인 페이지에 어떤 취약점이 있는지 알아보는 시나리오

![image-20220318222811102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318222811102.png)

- 관리자 계정 bee/bug 나 테스트 계정 test/test으로 로그인 해봐도 로그인 불가능

<br>

### 개발자 모드

![image-20220318222954493](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318222954493.png)

![image-20220318223026790](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318223026790.png)

- login과 password 레이블 옆에 로그인정보가 담겨있는데 로그인 해보면 성공..

<br>

<br>

## Insecure Login Forms - 난이도 : 중

![image-20220318223157693](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318223157693.png)

- Name에 brucebanner가 고정되어 있음
- 이 또한 개발자모드를 살펴보자

<br>

### 개발자 모드

![image-20220318223422048](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318223422048.png)

- 로그인 처리를 서버에서 하고 있지 않는 것을 확인할 수 있는데
- form에서 action 속성이 보이지 않기 때문이다,
- 다음으로 unlock_secret 이라는 사용자 정의 함수가 Unlock 클릭시 실행되고 있다.
- unlock_secret 함수를 알아보자

<br>

![image-20220318223647768](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318223647768.png)

- 자바스크립트 함수로서
- bash update killed my shells! 이라는 문자열에서 charAt(n) 함수를 이용해 n번째 문자를 추출하고 있다.
  - charAt(n) : 문자열 객체의 함수로서, 인자로 주어진 인덱스에 위치한 문자를 반환한다.
- 개발자 모드 콘솔로 이동해 알아보자

<br>

### Console

![image-20220318223956617](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318223956617.png)

![image-20220318224018653](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224018653.png)

- secret 알아내어 로그인 성공

<br>

<br>

## Insecure Login Forms - 난이도 : 상

![image-20220318224101705](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318224101705.png)

- 개발자모드에서 확인해보면 ID와 PW의 노출은 없었고 로그인에 대한 처리도 서버에서 구현하고 있다.