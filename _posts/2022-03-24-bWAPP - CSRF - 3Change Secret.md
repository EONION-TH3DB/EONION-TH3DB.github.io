---
title: "[bWAPP] 8. Cross-Site Request Forgery - Change Secret"
---

<br>

## Cross-Site Request Forgery

크로스 사이트 요청 변조는 공격자에 의해 사용자가 의도하지 않은 악의적인 행위를 서버에 요청하는 취약점.

공격자가 입력한 악의적인 스크립트 코드에 접근하면 사용자의 웹 브라우저는 스크립트 코드에서 의도한 내용을 웹 서버에 요청한다.

웹 서버는 변조된 요청을 사용자의 정상적인 요청으로 판단한여 응답을 보낸다.

<br><br>

## CSRF와  XSS의 차이

CSRF는 웹 브라우저를 신뢰해서 발생하는 취약점으로, 웹 사이트 사용자가 보낸 정상적인 요청과 변조된 요청을 구별하지 못한다.

또한, XSS는 공격 대상이 사용자이지만, CSRF는 사용자가 공격의 매개물로 공격자를 대신하여 악의적인 요청을 웹사이트에 요청한다.

<br><br>

## Change Secret

![image-20220325005938348](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325005938348.png)

- 현재 사용자의 비밀번호 힌트를 변경하는 시나리오이다.

<br>

### 개발자 도구(F12)

![image-20220325010115429](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325010115429.png)

- hidden 타입으로 선언된 login 변수가 노출되어 있고, login 변수에 현재 사용자의 아이디가 노출되어 있는 것으로 보아 아이디 값을 입력받는 변수라고 추측할 수 있다.

<br>

### BurpSuite

![image-20220325010305388](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325010305388.png)

- 또한, 버프수트를 통해 Change 버튼을 눌러 패킷을 캡쳐해보면 secret 변수, login 변수 action 변수를 확인할 수 있다.
- secret 은 새로운 비밀번호 힌트를 입력받고, login 변수는 현재 사용자의 아이디, action은 버튼의 동작을 입력받는 변수이다.
- 그렇다면 다음과 같은 이미지 태그 삽입공격을 해보자

```html
<img src= "http://172.30.1.16/bWAPP/csrf_3.php?secret=Eonion is King&login=bee&action=change" height= "0" width= "0">
```

<br>

### xss_stored_1.php

![image-20220325010636287](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325010636287.png)

<br>

### sqli_16.php

![image-20220325010713721](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325010713721.png)

- 성공적으로 변경된 것을 확인할 수 있다.

<br>

<br>

## 대응방안

### Linux

![image-20220325010901554](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325010901554.png)

- vi 편집기를 통해 csrf_3.php 소스 코드를 열어주자.
- 현 시나리오에서는 난이도와 상관없이 모든 변수에 다른 공격을 시도하지 못하게 mysqli_real_escape_string 함수로 입력 값을 우회한다.
- mysqli_real_escape_string
  - SQL 문법에서 사용되는 특수 문자 들에 백슬래시를 붙여 입력 값을 SQL 문법으로 인식하지 않게 방어한다. 
  - 백 슬래시가 붙는 문자는 NULL, \n, \r, \, ', ", ^Z이다
- 특히 secret 변수는 htmlspecialchars 함수를 호출하여 입력 데이터를 UTF-8로 인코딩하고, 두번째 인자에 ENT_QUOTES를 추가하여 XSS에 사용되는 특수 문자들을 HTML 엔티티 코드로 변환한다.
- 따라서 스크립트 코드가 입력될 경우 문자 그대로 인식하게 되어버린다.

<BR>

![image-20220325011234404](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325011234404.png)

![image-20220325011340009](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325011340009.png)

- 난이도 중, 상에서는 로그인 요청을 토큰으로 받기 때문에 사용자 비밀번호 힌트를 변경하기가 어렵다.
- 토큰을 생성할 때 랜덤 함수를 사용하는데, 비밀번호 힌트 변경을 요청할 때마다 새로운 토큰을 발행하여 토큰을 예측하거나 중복 사용하지 않게 방어한다.
- 토큰 값에 사용된 'mt_rand' 함수는 '메르센 트위스터'라는 알고리즘으로, 보통 사용하는 'rand' 함수보다 중복 값이 발생할 확률이 낮다.
- 또한, uniqid 함수를 통해 현재 시각을 사용하여 구별되는 식별자를 문자열 객체로 반환하고 있다.
- uniqid 함수의 반환 값은 중복되거나 예측할 수 있는 값을 생성할 가능성이 높지만, mt_rand 함수의 반환 값을 인자로 받기 때문에 중복되거나 예측할 확률이 매우 낮다.

<br>

<br>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습
