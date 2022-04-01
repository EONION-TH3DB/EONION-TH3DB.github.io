---
title: "[CodeEngn] Basic RCE L13"
---

<br>

## L13

### 문제

정답을 구하시오

<br>

### 파일 실행

![image-20220401160657463](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160657463.png)

<br>

### ExeinfoPE

![image-20220401160726417](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160726417.png)

<br>

### .NET Reflector

![image-20220401160800529](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160800529.png)

- Console.ReadLine : 입력
- Console.WriteLine : 출력
- RijndaelSimple.Encrypt : 위 String으로 선언한 변수들을 암호화
- RijndaelSimple.Decrypt : 다시 복호화
- plainText : RijndaelSimple.Decrypt 값 받음 / 즉, 암호 값

<br>

### Visual Studio로 열기

![image-20220401160836159](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160836159.png)

<br>

### 경로 지정

![image-20220401160849173](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160849173.png)

<br>

### 수정

![image-20220401160909888](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160909888.png)

- 정답인 암호 값(plainText) 출력 코드 추가
- F5 실행

<br>

### 결과

![image-20220401160935581](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401160935581.png)