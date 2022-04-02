---
title: "[CodeEngn] Basic RCE L17"
---

<br>

## L17

### 문제

![image-20220402182050781](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402182050781.png)

<br>

### 파일 실행

![image-20220402182105130](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402182105130.png)

- 한 글자 입력하니 오류 확인

<br>

### 올리디버거

![image-20220402182622821](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402182622821.png)

### ExeInfoPE

![image-20220402183122093](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183122093.png)

### All referenced text strings

![image-20220402183149817](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183149817.png)

- 한 글자 오류 확인하기 위해 해당 문자열 Double Click

<br>

![image-20220402183220483](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183220483.png)

- 위 문자열과 밑 문자열 비슷한 패턴
- CALL 17.0043A074 을 통해 EAX에 문자열 갯수 반환하고
- CMP EAX, 3 / CMP EAX, 1E 로 문자열 갯수 비교
  - CMP EAX,3 때문에 오류 발생
- 마지막에 CALL 17.0043A0A4로 EAX 값 문자열 갯수로 초기화

<br>

![image-20220402183359798](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183359798.png)

- 1로 수정

### All referenced text strings

![image-20220402183520202](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183520202.png)

- 분기점으로 이동

분기점

![image-20220402183638696](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402183638696.png)

- JNZ SHORT 17.0045BBC5 찾음

- CALL 함수에 BP

- F8 실행하면서 각각 어떤 역할 하는지 확인

  - CALL 17.0043A074 : EAX에 문자열 갯수 반환
  - CALL 17.0045B850 : 문자열에 따른 KEY 값 변환 알고리즘

  ![image-20220402184030147](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402184030147.png)

  - CALL 17.00404C3C : 문자열에 따라 변환된 KEY 값과 입력한 KEY 값 비교후 분기함수에 전달

  ![image-20220402184425855](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402184425855.png)

<BR>

### CALL 17.0045B850 - F7

![image-20220402184537855](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402184537855.png)

### F8로 진행

![image-20220402184703808](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402184703808.png)

- 해당 알고리즘 EDX와 ESI에서 문자열에 따라 변환된 KEY 값의 앞 4자리 값 발견
- 알고리즘을 코드로 변환시켜 Brute Force(무작위 대입)으로
- 문제에서 알려준 BEDA-2F56-BC4F4368-8A71-870B의 앞 4자리
- BEDA 찾기

### 해당 알고리즘 파이썬으로 변환

![image-20220402184823572](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402184823572.png)

- MOVZX ESI, BYTE PTR DS : [EBX+ECX-1] :: ESI = i
  - ESI에 문자열의 아스키 값 입력
  - 0~9 : 30~39 / A~I : 41~49 / a~I : 61~69
- ADD ESI, EDX :: 값 맞춰주기 위한 것임으로 코드에서 생략
- IMUL ESI, ESI, 772 :: ESI *= 0x772
- MOV EDX, ESI :: EDX = ESI
- IMUL EDX, ESI :: EDX *= ESI
- ADD ESI, EDX :: ESI += EDX
- IMUL ESI, ESI, 474 :: ESI *= 0x474
- ADD ESI, ESI :: ESI += ESI
- MOV EDX,ESI :: EDX에 옮겨담으므로 생략

<br>

### Python 코드

![image-20220402185033568](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402185033568.png)

- 70번 아스키 값 : F
- 정담 : F의 MD5 해쉬 값