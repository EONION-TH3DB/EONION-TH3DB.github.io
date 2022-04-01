---
title: "[CodeEngn] Basic RCE L11"
---

<br>

## L11

### 문제

basic 09와 같음

OEP + Stolenbyte 구하라

<br>

### 파일 실행

![image-20220401153045655](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153045655.png)

![image-20220401153054657](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153054657.png)

<br>

### 올리디버거

![image-20220401153154076](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153154076.png)

<br>

### ExinfoPe

![image-20220401153207038](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153207038.png)

<br>

### 언패킹

![image-20220401153249220](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153249220.png)

- F8 - Follow in Dump

<br>

![image-20220401153332144](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153332144.png)

- Dump Window - Breakpoint - Hardware on access - Dword

<br>

### F9

![image-20220401153418584](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153418584.png)

- POPAD 발견
- 분기 함수 전에 수상한 코드 발견

<br>

### F9

![image-20220401153538837](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153538837.png)

<br>

### OEP로 넘어가기

![image-20220401153606984](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153606984.png)

<br>

### F8 - Messagebox 진행

![image-20220401153639113](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153639113.png)

- 실행 시 파라미터 3개 누락 확인
- NOP 코드 총 12byte 확인

<br>

### 코드, 덤프, 스택 윈도우 대조

![image-20220401153702114](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153702114.png)

1. Code Window

   a. Messagebox 실행 전까지 파라미터 정상 입력

2. Register Window

   a. ESP - Follow in Dump

3. Stack Window

   a. Code address 0040100C : Stack address 0019FF64

   b. Stack address : 0019FF68 ~ 0019FF6B : Hex dump 12 20 40 00

   c. Stack address : 0019FF6C ~ 0019FF6F : Hex dump 00 20 40 00

   d. Stack address : 0019FF70 ~ 0019FF73 : Hex dump 00 00 00 00

4. 즉, Stolenbyte = 12byte

5. Code address 00401000 ~ 0040100B = Stolenbyte 자리

<br>

### POPAD

![image-20220401153820858](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153820858.png)

- Stolenbyte 발견
- OP CODE : 6A0068002040006812204000

<br>

### 언패킹

![image-20220401153843430](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153843430.png)

- Stolenbyte된 자리에 Ctrl + E 로 OP CODE 삽입

<br>

### OEP

![image-20220401153904874](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401153904874.png)

- OEP = 00401000