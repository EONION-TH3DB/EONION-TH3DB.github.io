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

![image-20220316230246276](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316230246276.png)

- Stored(Blog)와 똑같음

![image-20220316230425645](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316230425645.png)

- 작은 따옴표 하나 넣어봤더니 아무것도 안뜸

![image-20220316230452499](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316230452499.png)

- 두개 넣었더니 하나 뜸

![image-20220316230524693](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316230524693.png)

- 세개 넣으니 안뜸
- 즉, SQL 구문에 의해 ENTRY 구간이 작은 따옴표 하나로 열려있음을 알 수 있음
- 다시 유추해보면

`insert into tbl_name('#', 'Date', 'Entry', 'Owner')`

- 다음과 같이 형성 되어있을거라 판단
- 이제 저 Entry에 SQLite문을 삽입해보자.

`test', (select tbl_name from sqlite_master limit 0,1))--`

- test로 엔트리에 입력해주고 작은따옴표 닫아준 후 콤마를 찍어줘서 엔트리 조건을 충족시켜준 후에
- 바로 괄호를 열고 우리가 원하는 select 문을 삽입해준다.
- 그러고 괄호를 닫아주고 insert into tbl_name의 괄호도 충족시키기위해 하나 더 닫아주고
- 뒤에는 날려버릴거기에 주석으로 처리해준다

`insert into tbl_name('#', 'Date', 'test', (select tbl_name from sqlite_master limit 0,1))--', 'Owner')`

- 최종적으로는 이렇게 나올것이다.

<br>

### 테이블 조회

![image-20220316231214009](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316231214009.png)

`a', (select tbl_name from sqlite_master limit 6,1))--`

- 이런식으로 limit를 활용해서 조회해보자
  -  limit n,m ->  n번째의 m개만 출력이라는 뜻

<br>

### 컬럼 조회

![image-20220316231341229](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316231341229.png)

`a', (select sql from sqlite_master where tbl_name='heroes' limit 0,1))--`

<br>

### 결과 

![image-20220316231448404](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316231448404.png)

`a', (select login from heroes limit 0,1))--`

- 성공적으로 개인정보 탈취

<br>

<br>

## 대응방안

### Linux

![image-20220318001547842](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318001547842.png)

- vi 편집기로 sqli_12.php 소스코드 열어준다.
- Level 1, 2 = Medium, High 에서 sqli_check_4 함수 사용

<br>

### functions_external.php

![image-20220318001942380](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318001942380.png)

- 홀로있는 작은 따옴표를 두개의 작은 따옴표로 대체해준다. 
- 작은 따옴표를 열어줌과 동시에 닫아줌으로써 코드가 삽입되더라도 실행되지 못하게 Injection에 대해 방어하고 있다.

