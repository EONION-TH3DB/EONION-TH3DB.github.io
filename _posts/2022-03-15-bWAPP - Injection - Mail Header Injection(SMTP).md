---
title: "[bWAPP] 1. Injection - Mail Header Injection(SMTP)"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## SMTP

인터넷에서 메일을 주고 받기 위한 전송규약 및 프로토콜

<br>

<br>

## Mail Header Injection

![image-20220316014843359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316014843359.png)

- "bwapp@mailnator.com으로 너의 질문을 이메일 보내라" 라고 하네요

<br>

### 작성

![image-20220316015014801](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316015014801.png)

- 이런식으로?

<br>

<br>

## object

문제의 의도는 공격자가 메시지에 추가헤더를 삽입해 메일 서버가 의도한 것과 다르게 동작하기 위한 것

이메일 헤더에 참조표시인 cc나 숨은 참조 Bcc를 추가해 위에 보낸느 메일을 다른 메일 주소로 참조해 메일을 보내보자

<br>

### 개발자모드(F12)

![image-20220316015344036](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316015344036.png)

- input 형식을 textarea로 바꿔보자

![image-20220316015423667](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316015423667.png)

<br>

### 스크립트 삽입

![image-20220316015600319](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316015600319.png)

- Name으로 입력한 test 뒤에 엔터키(/r/n)을 입력하고 참조 Cc(Bcc도 상관없음):메일주소를 입력해 줬다.

  `test /n Cc:test@gmail.com`

<br>

### 결과

![image-20220316015824123](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316015824123.png)

- bwapp@mailnator.com 뿐만아닌 test@gmail.com 에게도 메일이 감
- `test /n Cc:test@gmail.com,test1@gmail.com,test2@gmail.com`
- 이런식으로 여러 계정에다 메일 보낼 수 있음

<br>

<br>

## 대응방안

### Linux

![image-20220317225020763](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317225020763.png)

- vi 편집기로 maili.php 소스코드를 열어보면
- High 레벨 일 때만 mail_check_2 함수를 사용한다.

<br>

### functions_external.php

![image-20220317225124474](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317225124474.png)

- 먼저 받은 데이터를 url디코딩 해주고 filter_var 함수로 유효성 검사를 해주고 있다.
- filter_var('검사할 값', Filter_Validate_Email);
  - 이 함수의 두번째 아규먼트 값에 따라 검사할 유형이 달라지는 방식
  - 두 번째 아규먼트 값은 이미 선언된 상수 값이기에 따로 선언할 필요 x
  - 해당 이메일이 올바르다면 참 값, 올바르지 않다면 거짓값을 반환한다.
