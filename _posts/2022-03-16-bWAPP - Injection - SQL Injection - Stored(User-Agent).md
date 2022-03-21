---
title: "[bWAPP] 1. Injection - SQL Injection - (User-Agent)"
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

## Injection - Stored

공격자가 서버(게시판)에 태그/스크립트를 저장시켜, 클라이언트가 게시판을 읽을 경우 태그/스크립트가 실행되는 방식.

이로 인해 쿠키 정보 탈취, 악성 코드 유입, 랜섬웨어 등의 피해로 이어질 수 있음.

<br>

<br>

## User-Agent

HTTP 요청 패킷의 헤더에 포함되는 내용

사용자 웹 브라우저에 대한 정보를 담고 있음

![image-20220316232157653](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316232157653.png)

- 버프 수트를 통해 알아보자

<br>

### BurpSuite

![image-20220316232332901](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316232332901.png)

- 조회한 후 Repeater로 보내보자

![image-20220316232527100](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316232527100.png)

- 먼저 Request에서의 User-Agent의 값을 작은 따옴표 하나로 정해두고 Send 버튼을 누르면
- Response에 응답이 뜨는데 SQL syntax 구문오류 표시가 뜨고 형광표시의 힌트가 뜬다.

![image-20220316232920223](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316232920223.png)

- 한 번 더 'test 로 입력해보면

`insert into table_name('Date', 'User-Agent', '&IP')`

- 다음과 같이 형성 되어있을거라 판단
- 이제 저 User-Agent에 SQL문을 삽입해보자.

`test', (select table_name from information_schema where table_schema='bWAPP' limit 0,1))#`

- test로 User-Agent에 입력해주고 작은따옴표 닫아준 후 콤마를 찍어줘서 User-Agent 조건을 충족시켜준 후에
- 바로 괄호를 열고 우리가 원하는 select 문을 삽입해준다.
- 그러고 괄호를 닫아주고 insert into table_name의 괄호도 충족시키기위해 하나 더 닫아주고
- 뒤에는 날려버릴거기에 주석으로 처리해준다

`insert into table_name(Date', 'test', (select table_name from information_schema where table_schema='bWAPP' limit 0,1))#', '&IP')`

- 최종적으로는 이렇게 나올것이다.

- 로 유추해볼 수 있다.
- 입력구문칸에 우리가 삽입할 코드를 삽입해주자.

<br>

### 테이블 조회

![image-20220316233142264](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316233142264.png)

`', (select table_name from information_schema.tables where table_schema='bWAPP' limit 3,1))#`

이런식으로 limit를 활용해서 조회해보자

-  limit n,m ->  n번째의 m개만 출력이라는 뜻

<br>

### 컬럼 이름

![image-20220316233830786](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316233830786.png)

​	<br>

### 결과

![image-20220316233924402](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316233924402.png)

- 성공적으로 개인정보 탈취

<br>

<br>

## 대응방안

### Linux

![image-20220318003429351](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318003429351.png)

![image-20220318002653266](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002653266.png)

- vi 편집기로 sqli_17.php 소스코드를 열어준다.
- level 1 : sqli_check_1 함수 사용
- level 2 : sqli_check_3 함수 사용

<br>

### functions_external.php

![image-20220318003522652](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318003522652.png)

- addslashes : 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 인코딩된 태그나 스크립트가 들어오면 바로 디코딩되기 때문에 보안 허점 발생
- mysql_real_escape_string : PHP에서 SQL Injection 공격 등을 방어하기 위하여 특수 문자열을 이스케이프 하기 위한 함수
  - `\x00, \n, \r, \, ', ", \x1a`와 같은 문자 앞에 `\`(역슬레시)를 붙여서 해당 문자가 실제 작동하지 않도록 이스케이프 해준다.
  - addslashes 보다 처리할 수 있는 문자들이 더 많음.
