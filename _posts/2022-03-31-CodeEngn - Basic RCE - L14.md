---
title: "[CodeEngn] Basic RCE L14"
---

<br>

## L14

### 문제

Name이 CodeEngn 일때 Serial을 구하시오

<br>

### 파일 실행

![image-20220401161701619](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401161701619.png)

- Name
- Serial

<br>

올리디버거

![image-20220401161744862](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401161744862.png)

- 패킹

<br>

ExeinFoPe

![image-20220401161803424](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401161803424.png)

<br>

언패킹(수동)

![image-20220401161826188](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401161826188.png)

<br>

분기점 찾기

![image-20220401161841510](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401161841510.png)

- EAX와 ESI 비교 후 성공 문자열 실패 문자열
- EAX, ESI 중 Serial 값 있을거라 추측
- CMP에 BP(Break Point)

<br>

GetDigItemTextA 함수

![image-20220401162014402](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401162014402.png)

- GetDigItemText : 컨트롤의 문자열을 얻는 함수
- BP 걸어줌

<br>

F9

![image-20220401162125043](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401162125043.png)

- GetDigItemText를 통해 Name과 Serial 호출
- 받은 문자열 길이만큼 EAX에 반환
- 길이가 0인지 체크해서 메시지박스 유무 판단

<br>

알고리즘

![image-20220401162556846](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401162556846.png)

- Name 값 길이 구함
- EAX(Name 길이) 만큼 알고리즘 반복
- 알고리즘(10진수 Seial 값) 결과값 ESI에 저장
- User 입력 Serial 값 CALL 14.00401383 으로 Serial 값 16진수로 변환후 EAX에 저장
- EAX와 ESI 비교 후 분기

<br>

결과

![image-20220401163403236](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401163403236.png)