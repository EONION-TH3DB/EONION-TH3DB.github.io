---
title: "[CodeEngn] Basic RCE L09"
---

<br>

## L09

### 문제

StolenByte를 구하는 문제

#### StolenByte

- 패킹된 바이너리를 언패킹할 때의 과정을 방해하기 위한 방법으로, 프로그램의 일부 바이트를 별도의 영역에서 실행되게 하여, OEP를 다른 위치로 가장하고 덤프를 쉽게 하지 못하도록 구현한 기법
- 패커가 패킹을 진행할 때, 원본 코드 중 일부를 다른 곳으로 이동 시킨 코드로써, 주로 엔트리 포인트 위의 몇 줄의 코드

https://croas.tistory.com/26

<br>

### 파일 실행

![image-20220401143059478](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143059478.png)

![image-20220401143111374](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143111374.png)

<br>

### 올리디버거 실행

![image-20220401143155120](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143155120.png)

- 패킹 상태로 추측

<br>

### ExeinfoPe

![image-20220401143210405](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143210405.png)

<br>

### UPX 언패킹

![image-20220401143231631](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143231631.png)

<br>

### 언패킹 파일 실행

![image-20220401143248805](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143248805.png)

- StolenByte가 문자열임을 확인

<br>

### 올리디버거(09.ex~ / 패킹파일)

![image-20220401143305822](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143305822.png)

- 코드 끝에 POPAD와 JMP 함수 확인
- 패킹 파일에서 JMP를 따라가면 언패킹 파일의 시작지점 OEP 도출

<br>

### 수동으로 언패킹(JMP 따라가기)

![image-20220401143524191](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143524191.png)

- PUSH 0 후 메시지박스 호출 확인
- MessageBox Function : 4개의 파라미터 필요

<br>

### 메시지박스 실행(0040100E)

![image-20220401143852351](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401143852351.png)

- 파라미터 PUSH 0 하나만 받았는데 정상적으로 실행

<br>

### 언패킹한 파일 올리디버거와 비교

![image-20220401144014275](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401144014275.png)

- JMP를 따라오며 수동으로 언패킹하기 전에 3개의 파라미터 선언하여 StolenByte가 적용됐을거라 추측

<br>

### PUSH 0 값의 스택포인트 확인

![image-20220401144206418](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401144206418.png)

![image-20220401144216406](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401144216406.png)

- 나머지 파라미터 값들 확인

<br>

### 언패킹 코드로 넘어가서 PUSH 값들 확인

![image-20220401144241732](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401144241732.png)

- POPAD에 브레이크 걸고 한단계 씩 진행
- PUSH 0 의 스택포인트 : 0019FF70
- PUSH 09.00402000 // : 0019FF6C
- PUSH 09.00402012 // : 0019FF68

<br>

### StolenByte

- 6A00 + 6800204000 + 6812204000