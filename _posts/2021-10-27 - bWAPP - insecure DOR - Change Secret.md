---
title: "[bWAPP] 4. Insecrue DOR - Change Secret"
---

# Definition 

## Direct Object References

서버 내부에 구현된 객체의 참조를 허용하는 것



## Insecure Direct Object References

주로 파일, DB Key, URL에 노출된 세션 아이디를 통해 조작 가능

접근 제어나 검증 절차가 없으면 공격자는 허가 없이 객체 참조를 조작하여 데이터에 접근 가능

# Change Secret

## Object

비밀번호 힌트를 수정할 수 있는 페이지에서 취약점을 알아보자



## 시나리오

### 난이도 : 하

![image-20211027221429986](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027221429986.png)

- 현 계정의 secret을 변경하는 폼

### 개발자 도구

![image-20211027221722764](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027221722764.png)

- ID를 입력하는 폼이 숨어있음
- 현 계정인 bee가 입력되어있는 상황
- 이를 조작해 특정 User의 계정의 secret을 변경해보자

### 변경

![image-20211027222053763](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027222053763.png)

![image-20211027222132696](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027222132696.png)

![image-20211027222235216](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027222235216.png)



## 확인

### A1. Injection - SQL Injection (Login Form/User)

변경 전

![image-20211027222357552](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027222357552.png)

변경 후

![image-20211027222519341](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027222519341.png)



## 대응방안

### 난이도 : 상

![image-20211027224113873](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027224113873.png)

- hidden 폼에 토큰 값 부여해서 수정 시 토큰 유효성 검사

### BeeBox

![image-20211027224349012](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027224349012.png)

- 디렉토리 변경
- vi 편집기로 해당 시나리오 소스코드 열기

### BeeBox - 1

![image-20211027224701299](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027224701299.png)

- Security Level 확인

### BeeBox - 2

![image-20211027224944471](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027224944471.png)

- mysql_real_escape_string : Null, \n, \r, \, ', ", ^Z 문자에 백슬래시를 붙여 SQL Injection 공격 방어
- htmlspecialchars : html에 사용되는 기호를 UTF-8로 반환하여 html 코드에 사용되는 문자들을 html 엔티티 코드로 반환 / 즉, 메타문자가 html 태그로 상요되지 않도록 방지하여 그대로 출력하게 함









