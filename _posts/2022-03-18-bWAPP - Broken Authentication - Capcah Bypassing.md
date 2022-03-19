---
title: "[bWAPP] 2. Broken Authentication - Capcah Bypassing"
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

## CAPTCHA Bypassing

![image-20220318165431057](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318165431057.png)

- 캡챠를 이용해 로그인을 시도하는 시나리오

![image-20220318165604196](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318165604196.png)

- 아무거나 입력해서 로그인하면 당연한 얘기지만 오류 구문이 뜬다.
- 저 오류 구문을 복사해뒀다가 진행할 Brute Force 기법에 활용하자.

<br>

### BurpSuite

![image-20220318170158936](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318170158936.png)

- 인터넷 옵션 - 연결 - LAN 설정 - 프록시 서버 사용 - 확인

<br>

![image-20220318170320718](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318170320718.png)

- 입력값을 넣어주고 Intercept is off -> intercept is on 해준 후 login 버튼을 누르면 위와 같이 패킷이 잡힌다.

![image-20220318170458852](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318170458852.png)

- 다음 intruder로 보내주면

<br>

![image-20220318170544249](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318170544249.png)

- Attack type은 Cluster Bomb로 바꿔주고
- 이제 페이로드를 실어줄건데, 현재 실려있는 모든 페이로드를 Clear & 해준 후 login과 password 값에만 실어준다.

![image-20220318170710536](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318170710536.png)

<br>

![image-20220318171125812](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171125812.png)

- Payloads로 이동해줘서

![image-20220318171207414](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171207414.png)

- 첫 번째 Simple list인 ID 값에 4개만 넣어준다.

![image-20220318171300843](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171300843.png)

- 다음은 두 번째 Simple list인 PW 값이 4개 넣어주고

<BR>

![image-20220318171351629](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171351629.png)

- Options로 와서

![image-20220318171447595](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171447595.png)

- Grep - Match에서 Clear 먼저 눌러주고
- 아까 복사해둔 로그인 실패시 뜨는 문구를 넣어준 후 우측 상단에 있는 start attack을 눌러준다.
- 무작위 공격에서 성공했을 때와 실패했을 때를 구분하기 위해서다.

<br>

### 결과

![image-20220318171739475](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318171739475.png)

- 성공하게 된 경우 Invalid credentials! Did you forgot your password? 구문 칸에 1 표시가 없는 것을 확인할 수 있으며 Response의 성공메시지 출력까지 확인할 수 있다.

<br>

<br>

## 대응방안

![image-20220318172543271](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318172543271.png)

- Medium(level = 1)과 High(level = 2)일 때와 Low일때의 차이를 보면
- Level 1, 2 는 Low에서 isset(&_SESSION['captcha'])의 AND 연산만 추가되어있다.
  - isset( &변수 ) 함수는 해당 변수가 설정되어 있는지 확인하는 함수로 설정되어있다면 참 그렇지않다면 거짓을 반환한다.
- 즉, 로우와 별 다를 것 없다.