---
title: "[CodeEngn] Basic RCE L02"
---

<br>

## L01

### 문제

패스워드 분석 문제

![image-20220401103626799](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103626799.png)

<BR>

### 파일 실행

![image-20220401103707284](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103707284.png)

<BR>

### 올리디버거 통해 실행

![image-20220401103728112](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103728112.png)

- 실행 파일 손상되어 실행할 수 없는 파일 확인

<BR>

### HxD : 헥스에디터 이용

![image-20220401103831994](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103831994.png)

- Offset 00000000 ~ 00000030 확인으로 실행파일 유무 판단
- 4D 5A로 시작 시 PE구조 가진 파일

<BR>

### 스크롤 내리기

![image-20220401103920363](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103920363.png)

- 우리에게 익숙한 함수 발견

<BR>

### 문자열 확인

![image-20220401103953969](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103953969.png)

- 비밀번호 확인