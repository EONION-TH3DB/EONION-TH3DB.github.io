---
title: "[Dreamhack] War Game 1단계 - Web cookie"

---

<br>

## 문제

![image-20211211191306786](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211191306786.png)



### 접속 정보의 URL

![image-20211211192935834](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211192935834.png)



### 다운로드 코드

![image-20211211192321001](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211192321001.png)

- 사용자는 guest와 admin이라는 것을 확인
- username인 사용자로 쿠키 값을 생성하는 것 확인
- 쿠키값이 guest일 때 you are not admin을 출력하고
- 쿠키값이 admin일 때는 FLAG 값을 출력하는 것 확인
- 그렇다면 guest 계정 정보를 알아낸 후 guest 계정에서 쿠키값을 admin으로 바꾸보자



### 개발자모드

![image-20211211193055771](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211193055771.png)

- 기본 계정의 정보 파악



### 로그인

![image-20211211193208495](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211193208495.png)

- guest로 로그인시 출력되는 메시지와 쿠키값 확인



### 쿠키값 수정

![image-20211211193300064](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211193300064.png)

- FLAG 값 획득

