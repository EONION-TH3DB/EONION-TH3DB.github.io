---
title: "[CodeEngn] Basic RCE L07"
---

<br>

## L07

### 문제

컴퓨터 C 드라이브의 이름이 CodeEngn 일경우 시리얼이 생성될때 CodeEngn은 'ß어떤것'으로 변경되는가

<br>

### 파일 실행

![image-20220401135801882](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135801882.png)

<br>

### 올리디버거

![image-20220401135855349](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135855349.png)

<br>

### All referenced text string

![image-20220401135923628](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401135923628.png)

- 이동

<br>

### 분기점 확인

![image-20220401140020840](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401140020840.png)

<br>

### BreakPoint걸고 실행

![image-20220401140104056](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401140104056.png)

<br>

### GetVolumeInformationA 함수를 통해 C드라이브 명칭 004010A3에 출력

![image-20220401140224144](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401140224144.png)

<br>

### lstrcatA 함수 통해 기존 ConcatString 뒤에 StringToAdd 문자열 추가

![image-20220401140407951](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401140407951.png)

<br>

### 반복문 진입

![image-20220401140428090](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401140428090.png)

- DL에 값 2 부여하고 004010CB에서 DL 값을 1씩 감소
- JNZ로 DL값이 1이면 참
- 즉 반복문 2번 시행

<br>

### 첫 번째 결과값

![image-20220401141840098](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401141840098.png)

- 40225C~F까지 1씩 더한 아스키 값에 해당하는 문자열 출력
- Code -> Dpef

<br>

### 두 번째 결과값

![image-20220401141925237](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401141925237.png)

- 40225C~F까지 1씩 더한 아스키 값에 해당하는 문자열 출력
- Dpef -> Eqfg

<br>

### lstrcatA 함수로 StringToAdd 값 출력

![image-20220401142025923](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401142025923.png)

<br>

### 한번 더 lstrcatA 함수로 StringToAdd 값 뒤에 이어서 출력

![image-20220401142127209](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401142127209.png)

<br>

### String2와 String1 값 비교 lstrcmpiA

![image-20220401142207377](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401142207377.png)

- 비교 통해 참 거짓 문자열 출력
- 즉, 시리얼 값은 String1 확인