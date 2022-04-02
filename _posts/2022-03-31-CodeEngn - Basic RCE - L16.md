---
title: "[CodeEngn] Basic RCE L16"
---

<br>

## L16

### 문제

Name이 CodeEngn 일때 Serial을 구하시오

<br>

### 파일 실행

![image-20220402180447237](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402180447237.png)

<br>

### ExeInfoPE

![image-20220402180517487](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402180517487.png)

<br>

### All referenced text string

![image-20220402180551837](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402180551837.png)

- 성공 및 실패 구문 찾아서 Double Click으로 이동

<br>

### 분기

![image-20220402180618292](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402180618292.png)

- 분기점 찾기
- CMP 함수에서 EAX값과 [EBP-3C] 주소 값을 비교하는데 하나는 우리가 입력한 Serial 값, 하나는 등록된 Serial 값으로 예상

<br>

### F9

![image-20220402180725058](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402180725058.png)

<br>

### EAX, EBP

![image-20220402181336932](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402181336932.png)

- 입력시 EAX값과 EBP값 확인

<br>

### Fallow in Dump - Memory address

![image-20220402181352208](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402181352208.png)

- EBP-3C 주소값을 찾아보자

<br>

### EBP - 3C

![image-20220402181407684](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402181407684.png)

<br>

### Dump Window

![image-20220402181454885](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402181454885.png)

<br>

### 변환

![image-20220402181521365](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220402181521365.png)

<br>

### 결과

10진수(EBP - 3C) = 3838184855