---
title: "[CodeEngn] Basic RCE L12"
---

<br>

## L12

### 문제

![image-20220401155119046](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155119046.png)

<br>

### 파일 실행

![image-20220401155150833](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155150833.png)

<br>

### 올리디버거

![image-20220401155213297](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155213297.png)

<br>

### All referenced text string

![image-20220401155306562](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155306562.png)

<br>

### 성공 문구 이동

![image-20220401155335156](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155335156.png)

- GetDigItemInt
  - 대화상자의 지정된 컨트롤의 텍스트를 정수 값으로 변환하는 함수
  - User가 입력한 값을 가져와 키 값과 비교하는 방식임을 예측 가능
  - BreakPoint를 걸어 User가 입력한 값이 어디에 저장되는지 확인하여 키 값 추출
- ESI에 특정 값 삽입
- EBX에 ESI값 넣고
- ESI에 4byte 씩 덧셈 연산
- 반복
- 결국 EAX값과 7A2896BF와 비교
  - 사용자 입력값을 정수 값으로 변환한 EAX값과 비교
  - 즉, 7A2896BF는 키 값임을 추론
- 7A2896BF는 10진수로 2049480383

<br>

### HexEditor

![image-20220401155408685](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155408685.png)

- Messagebox에 출력되는 문자열 확인
- 이 주소에 10진수 키 값 OverWrite

<br>

### OverWrite

![image-20220401155425986](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220401155425986.png)

- 주소 0D3B ~ 0D45 확인

<br>

### 결과

- Auth = 20494803830D3B0D45