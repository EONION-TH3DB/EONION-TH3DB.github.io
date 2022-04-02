---
title: "[CodeEngn] Basic RCE L18"
---

<br>

## L18

### 문제

![image-20220402185307933](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185307933.png)

<br>

### 파일 실행

![image-20220402185746106](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185746106.png)

<br>

### 올리디버거

![image-20220402185826780](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185826780.png)

<br>

### ExeInfoPE

![image-20220402185845406](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185845406.png)

<br>

### All referenced text strings

![image-20220402185857543](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185857543.png)

<br>

### Double Click

![image-20220402185919746](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185919746.png)

- wsprintfA : 문자열을 버퍼에 저장
- lstrcmpiA로 String1과 String2 비교하여 분기로 가는 걸로 추측

<br>

### F9

![image-20220402190140323](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402190140323.png)

- 상단의 wsprintfA는 CodeEngn 저장

<br>

![image-20220402190307354](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402190307354.png)

- 하단의 wsprintfA의 CALL함수를 통해 String2 값에 Name에 따른 Serial 값 저장

<br>

![image-20220402190322661](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402190322661.png)

- GetDigItemTextA로 사용자 입력값 String2에 삽입
- lstrcmpiA를 통해 비교 후 분기 도달

<br>

### Serial 값

![image-20220402190336366](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402190336366.png)