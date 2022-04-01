---
title: "[CodeEngn] Basic RCE L03"
---

<br>

## L03

### 문제

스트링 비교함수 찾는 문제

![image-20220401104257980](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104257980.png)

<BR>

### 파일 실행

![image-20220401104324449](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104324449.png)

![image-20220401104340762](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104340762.png)

- Regcode 입력시 Registrien 으로 확인

<BR>

![image-20220401104417502](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104417502.png)

- 스트링 비교 함수 있을거라 추측

<BR>

### 올리디버거

![image-20220401104518698](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104518698.png)

<BR>

### 해당 프로그램에서 사용하는 함수 보여주는 기능인 'All intermodular calls' 실행

![image-20220401104611283](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104611283.png)

- 더블 클릭 후 함수 사용 구역 확인
- 004028BA - 주소에 있는 double word PUSH
- 004028BD - 2G83G35Hs2 PUSH
- 004028C2 - PUSH한 두 문자열 비교

<BR>

### 결과

![image-20220401104650114](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401104650114.png)