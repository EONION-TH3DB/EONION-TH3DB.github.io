---
title: "[bWAPP] 1. Injection - SQL Injection - (Login Form/User)"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## SQL Injection

응용 프로그램 보안 상의 허점을 의도적으로 이용해, 

임의의 SQL문을 주입하여 악의적인 SQL문을 실행하게 함으로써

DB를 비정상적으로 조작하는 코드 인젝션의 대표적인 공격.

이로인해 공격자가 DB에 저장되어 있는 다른 사용자의 개인 정보 등 허가되지 않은 정보에 접근하여,

데이터 변조 및 조작 가능.

<br>

<br>

## Object

개인정보를 탈취해보자

![image-20220316200921850](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200921850.png)

![image-20220316200948293](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200948293.png)

- 되지 않는 것으로 보아 뭔가 있는 것 같다.

- 현재 웹 페이지 php 소스코드를 열어보자

  ![image-20220316201022413](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316201022413.png)

### sqli16.php

![image-20220316201800055](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316201800055.png)

### sqli3.php

![image-20220316201835621](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316201835621.png)

- Form/User - sqli16의 시나리오에서는 Form/Hero와 달리 로그인 시도시 144번 login 값만 넘겨주고
- Form/Hero - sqli3의 시나리오에서는 140번 login과 password를 같이 넘겨준다.
- 그렇기에 다시 Form/user 시나리오에서는 로그인값과 패스워드 값이 정답이어야 성공으로써 login에서의 sql 삽입공격 시도는 먹히지 않는걸 확인할 수 있다.
- SQLMAP을 이용해보자

<br>

### SQLMAP

![image-20220316202618966](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316202618966.png)

- 먼저 버프수트를 이용해 쿠키값 알아주고 다음과 같이 입력하여 sqlmap 실행

<br>

### DB확인

![image-20220316202507313](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316202507313.png)

![image-20220316202703481](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316202703481.png)

<br>

### 테이블 확인

![image-20220316202758647](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316202758647.png)

![image-20220316202929558](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316202929558.png)

<br>

### 컬럼 확인

![image-20220316203113580](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316203113580.png)

![image-20220316203136515](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316203136515.png)

<br>

### 결과

![image-20220316203233524](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316203233524.png)

![image-20220316203247080](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316203247080.png)

- 성공적으로 개인정보 결과 조회