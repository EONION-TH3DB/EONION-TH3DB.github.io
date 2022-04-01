---
title: "[CodeEngn] Basic RCE L01"
---

<br>

## L01

### 문제

HDD를 CD-Rom으로 인식시키는 GetDriveTypeA의 리턴값을 구하는 문제

<BR>

### GetDriveTypeA Function

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/Untitled.png" alt="img" style="zoom:50%;" />

- disk drive의 Type을 결정하는 함수

<BR>

### 파일 실행

![image-20220401101713791](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401101713791.png)

<BR>

### 확인 - Click

![image-20220401101921523](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401101921523.png)

<BR>

### 올리디버거로 파일 실행

![image-20220401102100697](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401102100697.png)

- 메시지 박스 확인 시 파일 실행 시 나왔던 문자열 확인
- 실패와 성공 문자열 동시에 확인

<BR>

### 리턴값 알아보기 위해 0040101D에 Break

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401102321815.png" alt="image-20220401102321815" style="zoom:120%;" />

- 레지스터 상태 영역에서 리턴값 확인

- 레지스터 용도

  <img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401102650979.png" alt="image-20220401102650979" style="zoom:50%;" />

- EAX 값 3으로 HDD Return 값 3 도출

<BR>

### 00401026까지 차례대로 실행(Step over - F8)

![image-20220401102827557](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401102827557.png)

- EAX 두번 내리고 ESI 세번 올린 후 JMP 조건 명령어 실행

- JMP 조건 명령어

  <img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401102905507.png" alt="image-20220401102905507" style="zoom:50%;" />

- 조건 플래그 값이 1(값이 같음)이면 성공 메시지 0(값이 다름)이면 오류 메시지 확인

<BR>

### 첫 번째 방법

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103323510.png" alt="image-20220401103323510" style="zoom:120%;" />

- 00401026 줄의 어셈블리어 명령 코드에서 JE를 JMP로 수정

<BR>

### 두 번째 방법

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103141532.png" alt="image-20220401103141532" style="zoom:120%;" />

- 00401024 00401026 줄의 어셈블리어 명령 코드에서 비교값 통일

<BR>

<BR>

### 결과

![image-20220401103246542](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401103246542.png)