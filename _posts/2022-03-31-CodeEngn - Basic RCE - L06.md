---
title: "[CodeEngn] Basic RCE L06"
---

<br>

## L06

### 문제

Unpack 한 후 Serial을 찾는 문제

<br>

### 파일 실행

![image-20220401134750094](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134750094.png)

### 패킹 확인

![image-20220401134838193](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134838193.png)

- UPX 패킹 확인

언패킹

![image-20220401134910096](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134910096.png)

올리디버거 실행

![image-20220401134925428](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134925428.png)

- OEP(Original entry point) = 00401360

All referenced text strings

![image-20220401135042293](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135042293.png)

- Check Serial 실패 문자열 확인
- 이동(Double Click)

Break Point

![image-20220401135117452](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135117452.png)



### 반환값 비교

![image-20220401135229650](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135229650.png)

- EAX = 1;

<BR>

![image-20220401135345808](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135345808.png)

- EAX = 0;

<BR>

### 질문

- EAX = FFFFFFFF; 질문