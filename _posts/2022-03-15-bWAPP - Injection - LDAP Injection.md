---
title: "[bWAPP] 1. Injection - LDAP Injection"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## LDAP

Lightweight Directory Access Protocol

네트워크 상에서 조직이나 개인정보 혹은 파일이나 디바이스 정보 등을 찾아보는 것을 가능하게 만든 소프트웨어 프로토콜

![image-20220316014205132](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316014205132.png)

![image-20220316014328756](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316014328756.png)

- SET 누르면 다음과 같이 뜸

<BR>

### 개발자 모드(F12)

![image-20220316014451413](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316014451413.png)

- Head 쪽 확인 결과 성공시 뜨는 주소 발견

<BR>

### 그대로 삽입

![image-20220316014557731](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316014557731.png)

- 뭐 어찌저찌 해결은 됨...

<br>

<Br>

### 대응방안

### Linux

![image-20220317224705099](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317224705099.png)

- 따로 보안적용이 되어 있지 않다.