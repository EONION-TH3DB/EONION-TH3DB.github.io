---
title: "[CodeEngn] Basic RCE L05"
---

<br>

## L01

### 문제

이 프로그램의 등록키는 무엇인가

<br>

### 파일 실행

![image-20220401133043535](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133043535.png)

<br>

### Register now

![image-20220401133116520](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133116520.png)

<br>

### Register now 2

![image-20220401133145438](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133145438.png)

- 이름이 들어가는지 안들어가는지 체크하는 부분과
- 입력된 시리얼이 참인지 비교하는 구문 있을거라 추축

<br>

### 올리디버거

![image-20220401133255198](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133255198.png)

- PUSHAD를 통해 UPX 패킹되어있을거라 추측

<br>

### EXEINFO PE

![image-20220401133320105](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133320105.png)

- UPX 패킹 확인

<br>

### UPX 패커로 언패킹

![image-20220401133349621](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133349621.png)

<br>

### 올리디버거 : 언패킹파일

![image-20220401133426785](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133426785.png)

<br>

### 코드에서 참조되는 문자열 보기위해 Search for → All referenced text strings

![image-20220401133456375](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133456375.png)

- 성공 문구 발견
- 성공 문구 위에 이름과 등록키로 추정

<br>

### 성공 문구 이동(Double Click)

![image-20220401133626962](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133626962.png)

- BreakPoint 잡고 임의 입력 후 Register now!

<br>

### 한 단계 진행 : F8

![image-20220401133728748](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133728748.png)

- EAX에 입력한 eonion

<br>

### F8

![image-20220401133826796](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133826796.png)

- EDX에 Register User

<br>

### 00440F34 내부 : F7

![image-20220401133856006](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401133856006.png)

- EAX와 EDX 비교
- 실패 메시지 출력 -> 즉 ID는 Registered User

<br>

### Register User 입력

![image-20220401134155148](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134155148.png)

- 다시 처음부터 똑같이 진행해서 ID 칸에 Register User 를 입력해주자.
- CMP하여 참값을 반환

<br>

### 등록키 비교하는 두번째 BreakPoint로 이동

![image-20220401134409653](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134409653.png)

- 00440F51 : 00440F34와 같은 함수를 쓰는 내부 구조라는 걸 파악
- 즉, 등록되어있는 값과 입력한 값을 비교하여 참 거짓 값을 반환하는 함수
- EAX에는 입력한 값, EDX에는 등록되어있는 등록키가 보이는 것 확인

<br>

### 결론

![image-20220401134554093](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401134554093.png)

- 유저 네임과 등록키 둘 다 올바르게 입력해야 성공 메시지 출력