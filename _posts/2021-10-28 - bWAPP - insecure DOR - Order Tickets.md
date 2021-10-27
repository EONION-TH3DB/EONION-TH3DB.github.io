---
title: "[bWAPP] 4. Insecrue DOR - Order Tickets"
---

# Definition 

## Direct Object References

서버 내부에 구현된 객체의 참조를 허용하는 것



## Insecure Direct Object References

주로 파일, DB Key, URL에 노출된 세션 아이디를 통해 조작 가능

접근 제어나 검증 절차가 없으면 공격자는 허가 없이 객체 참조를 조작하여 데이터에 접근 가능

# Order Tickets

## Object

- Insecure DOR을 통해 티켓의 가격을 조작하자



## 시나리오

### 난이도 : 하

![image-20211028000718248](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028000718248.png)



### 개발자 도구

![image-20211028001241283](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028001241283.png)

- hidden 타입으로 보이지 않음
- type text, 값 10으로 수정

### 개발자 도구 - 1

![image-20211028001450524](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028001450524.png)

![image-20211028001513601](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028001513601.png)



## 확인

변경 전

![image-20211028001631609](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028001631609.png)

변경 후

![image-20211028001612409](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028001612409.png)



## 대응방안

### 난이도 : 상

개발자 모드

![image-20211028002742673](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028002742673.png)

- 티켓 가격 코드 확인 불가
- 서버측에서 값을 받아와 DB에 전달될 때까지 사용자가 수정을 못하도록 대응

