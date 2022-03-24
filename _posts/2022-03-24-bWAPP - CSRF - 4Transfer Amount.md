---
title: "[bWAPP] 8. Cross-Site Request Forgery - Transfer Amount"
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

## Transfer Amount

![image-20220325011800866](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325011800866.png)

- 현 시나리오는 계좌 이체 기능을 제공한다.
- 현 계좌에 있는 돈은 1000유로인데 CSRF를 통해 계좌에 있는 돈을 수정해보자.

<br>

![image-20220325012059383](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325012059383.png)

- Transfer 를 눌러보면 GET 메서드로 요청하는 페이지이므로 URL에 변수가 나타나는 것을 확인할 수 있다.

<BR>

### 개발자도구(F12)

![image-20220325011945733](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325011945733.png)

- account 변수와 amount 변수가 노출된 것을 확인할 수 있는데, account 변수는 계좌번호 amount 변수는 이체할 금액을 의미한다.
- 그렇다면 이 또한 이미지 태그 삽입 공격을 진행해보자
- amount 변수 값에 마이너스 값을 입력하여 계좌의 돈을 증가하게 하는 코드이다.

```html
<img src= "http://172.30.1.16/bWAPP/csrf_2.php?account=123-45678-90&amount=-100&action=transfer" height= "0" width= "0">
```

<br>

### xss_stored_1.php

![image-20220325012531326](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325012531326.png)

![image-20220325012624593](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325012624593.png)

- 조작한 이미지 태그가 있는 블로그에 사용자들이 접근할 때마다 123-45678-90에 있는 계좌의 돈은 100유로 씩 증가한다..

<br>

<br>

## 대응방안

### Linux

![image-20220325013351262](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325013351262.png)

- vi 편집기를 통해 csrf_2.php 소스 코드를 열어주자.
- 난이도 하 에서 계좌 이체는 생성한 토큰이 일치할 때만 가능한데, 중복된 토큰을 사용하지 못하므로 계좌번호를 변경하지 못한다.
- 그러나 이체할 금액에 마이너스 값은 입력할 수 있으므로 계좌에 있는 금액이 더 늘어나지 않게 방어해야 한다.

<br>
![image-20220325013407952](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325013407952.png)

![image-20220325013523845](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325013523845.png)

- 난이도 상에서는 토큰을 사용하므로 변수와 함께 히든타입으로 토큰 값이 들어가 있다.
- 세션 할당을 위해 mt_rand 함수로 랜덤 값을 반환하고 다시 그 반환 값을 uniqid 함수의 인자로 입력한다.
- 그 다음 SHA-1 함수를 통해 uniqid의 반환 값을 해시 값으로 생성하여 토큰 값으로 사용한다.
- 토큰 값에 사용된 'mt_rand' 함수는 '메르센 트위스터'라는 알고리즘으로, 보통 사용하는 'rand' 함수보다 중복 값이 발생할 확률이 낮다.
- 또한, uniqid 함수를 통해 현재 시각을 사용하여 구별되는 식별자를 문자열 객체로 반환하고 있다.
- uniqid 함수의 반환 값은 중복되거나 예측할 수 있는 값을 생성할 가능성이 높지만, mt_rand 함수의 반환 값을 인자로 받기 때문에 중복되거나 예측할 확률이 매우 낮다.

<br>

<br>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습