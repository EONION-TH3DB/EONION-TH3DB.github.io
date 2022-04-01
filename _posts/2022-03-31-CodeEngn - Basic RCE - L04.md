---
title: "[CodeEngn] Basic RCE L04"
---

<br>

## L01

### 문제

이 프로그램은 디버거 프로그램을 탐지하는 기능을 가지고 있음

디버거를 탐지하는 함수의 이름은 무엇인가

### 파일 실행

![image-20220401105132012](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105132012.png)

<br>

### 올리디버거 실행

![image-20220401105208501](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105208501.png)

<br>

### All intermodular calls

![image-20220401105228425](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105228425.png)

<br>

### 해당 위치로 이동

![image-20220401105308443](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105308443.png)

- BreakPoint 잡아주기(Double Click)

<br>

### 실행(Run : F9)

![image-20220401105419799](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105419799.png)

- BreakPoint의 함수 실행되고 난 후 반환값 받고 다음 줄에 있는 CMP 명령을 실행하고
- 비교되는 값에 따라 '정상', '디버깅 안함' 이라는 문자열 출력될 걸로 추측

<br>

### F8 : 한 줄씩 Step Over

![image-20220401105539684](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105539684.png)

- BreakPoint 실행 후 EAX 값 1 반환
- 0040106F : 00431024의 값을 Arg1 인자로 받음

<br>

### Dump Window에 00431024 값 찾기

![image-20220401105929623](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401105929623.png)

- 00401074 : 00408190 함수를 호출하는 데 이 함수는 문자열 출력하는 기능 확인

<br>

### 무한 루프 발견

![image-20220401113247492](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401113247492.png)

- 00401048 : ESP 값을 읽어들여 ESI에 복사
  - MOV 명령어 : MOV [복사될 곳], [읽어들일 곳]
- 0040104A ~ 0040104F: 1초 쉬고
- 0040105C : ESP 값을 읽어들여 ESI에 복사
- 0040105E : 디버그 모드인지 판별
- 0040106B : EAX 값이 0인지 조건
  - TEST 명령어
    - AND 비트 연산 수행
    - SF, ZF, PF 플래그 수정되며 AND 결과는 버려짐(CMP와 같이 저장되지 않음)
    - 플래그 값을 세팅하여 JE, JZ와 같은 분기문에 영향
    - ex) TEST EAX, EAX
      - 논리상 레지스터끼리 연산 시 EAX 값 도출
      - EAX 값이 0인지 아닌지를 판단하기 위함
- 0040106D : 참이면 7E, 거짓이면 6F
- 0040108B : 00401048로 이동

<br>

### IsDebuggerPresent 내부 : BreakPoint에서 F7 실행

![image-20220401114039036](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401114039036.png)

- 75DE9F20 : 'FS:[30]' 값을 EAX에 복사
  - 'FS' : 세그먼트 레지스터
  - 'FS[0x30] 4 INT' : Process Environment Block (PEB)의 선형 주소
  - '[30]' : Pointer ro PEB
    - PEB : 윈도우 NT에서의 데이터 구조체
  - PEB 구조를 통해 안티 디버깅 가능
  - 'FS : [30]'으로 디버깅 모드인지 아닌지 0, 1로 다음 주소에 반환
- 75DE9F26 : EAX 레지스터 값에 2를 더한 주소값을 EAX에 복사
  - EAX = 003BA000
  - EAX + 2 = 003BA002 = 01
  - 01 값 반환
- 75DE9F2A : RETN 값 00000001

<br>

### 결론

- BreakPoint에서의 EAX의 반환값이 00000001
- 무한루프에서 TEST EAX, EAX로 EAX값이 0인지 조건
- 거짓으로 '디버깅 당함'이라는 문자열로 디버깅 모드임을 알려줌