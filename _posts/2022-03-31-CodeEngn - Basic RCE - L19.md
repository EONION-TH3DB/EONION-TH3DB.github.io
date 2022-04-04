---
title: "[CodeEngn] Basic RCE L19"
---

<br>

## L19

### 문제

![image-20220404103744591](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404103744591.png)

<br>

### 파일 실행

![image-20220404103859728](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404103859728.png)

<br>

### ExeInfoPE

![image-20220404103914732](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404103914732.png)

<br>

### 올리디버거

![image-20220404104104303](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104104303.png)

<br>

### 언패킹

![image-20220404104121324](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104121324.png)

- 수동으로 언패킹한 부분에 timeGetTime 함수 누락된 부분이 있어서 자동으로 권장

<br>

### OEP - F9

![image-20220404104136465](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104136465.png)

- 다른 메시지 박스(오류)

<br>

### F8 진행해서 오류 부분 찾기

![image-20220404104242368](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104242368.png)

- 오류 발견
- bp 설정

<br>

### F7로 CALL 19.0040EA50 함수 내부 접속

![image-20220404104311717](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104311717.png)

<br>

### F8 진행

![image-20220404104401550](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104401550.png)

- 오류 발견
- BP 설정

<br>

### F7로 CALL 19.0040E940 함수 내부 접속

![image-20220404104427315](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104427315.png)

- lsDebuggerPresent 함수 발견  : Anti - Debugging 기법으로 디버깅을 당하면 1, 당하지 않으면 0 반환
- JNZ 함수에 ZF=0을 반환하여 Operand로 점프
  - ZF 제로 플래그는 연산 결과(TEST EAX, EAX)가 0이면 참(1)
    - TEST EAX,EAX 는 EAX의 값이 0이냐 아니냐를 판단하는 구문
  - JZ는 제로 플래그가 참이면(연산 결과 = 0) Operand로 점프
  - JNZ는 제로 플래그가 거짓이면(연산 결과 ≠ 0) Operand로 점프

<br>

### 0040E969 = 19.004338DE

![image-20220404104758679](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104758679.png)

- 오류 발견 -> 이쪽으로 오지 못하게 하자.

<br>

### 코드 수정

![image-20220404104846467](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104846467.png)

- JNZ → JE 로 수정

<br>

### Search for - All Intermodular calls - SetTimer

![image-20220404104952629](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404104952629.png)

![image-20220404105003694](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404105003694.png)

- 너무 짧은 750ms, 프로그램 종료와 상관없는 부분으로 패스

<br>

### Search for - All Intermodular calls - timeGetTime

![image-20220404105049637](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404105049637.png)

- BP 설정

<br>

### F9

- timeGetTime : systemtime을 ms 단위로 반환하는 함수
- 444C44 ~ 444C4D : 첫 timeGetTime 함수를 호출 할 때의 systemtime을 ESI에 저장
- 444C5F ~ 444C61 : 다시 timeGetTime 함수 호출한 값 EAX에 저장후 ESI와 비교
- CMP EAX, ESI : EAX값이 ESI 보다 크면 444D38로 분기

<br>

### JNB 19.00444D38

![image-20220404105658662](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404105658662.png)

- EAX에서 ESI 뺀 값과
- EBX+0x4와 비교
- EAX가 더 크면 00444C71로 분기하여 종료
- 더 작으면 00444C5F로 분기하여 EAX가 더 커져 종료될 때까지 반복

<br>

### 결론

![image-20220404110054760](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110054760.png)

![image-20220404110113421](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110113421.png)

![image-20220404110130134](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110130134.png)

- EAX(timeGetTime을 호출한 현재시간) - ESI(첫 timeGetTime system시간) > EBX + 0x4
- EBX + 0x4 = 00002B70
- 00002B70 = 11120ms / 11.12초

<br>

### Search for - All Intermodular calls - Sleep

![image-20220404110308015](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110308015.png)

![image-20220404110318027](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110318027.png)

- 상동