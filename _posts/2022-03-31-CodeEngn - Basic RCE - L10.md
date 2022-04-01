---
title: "[CodeEngn] Basic RCE L10"
---

<br>

## L10

### 문제

OEP를 구한 후 '등록성공'으로 가는 분기점의 OPCODE를 구하시오

정답 = OEP + OPCODE

<br>

### 파일 실행

![image-20220401145734778](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401145734778.png)

<br>

### 올리디버거

![image-20220401145806683](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401145806683.png)

- 패킹된 모습 파악

<br>

### exeinfope

![image-20220401145826998](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401145826998.png)

- Aspack로 패킹된 형태 파악

<br>

### 언패킹 원리

1. PUSHAD를 통해 스택에 레지스터 적재
   1. 이 지점에서 ESP에 접근하는 주소에 브레이크
2. 메모리에 원본 코드 복구
3. POPAD를 통해 스택으로부터 레지스터 추출 → 참조 시작

<br>

### F8

![image-20220401145944006](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401145944006.png)

<br>

### Follow in dump

![image-20220401150007944](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150007944.png)

<br>

### 확인

![image-20220401150033218](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150033218.png)

<br>

### Breakpoint - Hardware on access - Dword

![image-20220401150052299](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150052299.png)

<br>

### F9 - F8

![image-20220401150140492](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150140492.png)

- POPAD
- OEP로 이동하기 전 분기점

<br>

### F8(OEP로 이동)

![image-20220401150157518](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150157518.png)

<br>

### Ctrl + A

![image-20220401150216605](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150216605.png)

- 언패킹
- OEP = 00445834

<br>

### Search for - All referenced text strings

![image-20220401150239130](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150239130.png)

- 성공시 문자열 발견
- 분기점 있을거라 확신

<br>

### 이동

![image-20220401150432550](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401150432550.png)

- 분기 명령어 발견
- 분기점의 OPCODE = 75 55
