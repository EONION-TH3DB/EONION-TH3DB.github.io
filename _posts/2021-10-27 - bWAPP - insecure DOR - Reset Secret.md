---
title: "[bWAPP] 4. Insecrue Direct Object References - Reset Secret"
---

# Definition 

## Direct Object References

서버 내부에 구현된 객체의 참조를 허용하는 것



## Insecure Direct Object References

주로 파일, DB Key, URL에 노출된 세션 아이디를 통해 조작 가능

접근 제어나 검증 절차가 없으면 공격자는 허가 없이 객체 참조를 조작하여 데이터에 접근 가능

# Change Secret

## Object

비밀번호 힌트 변경 요청 없이 초기화할 비밀번호 힌트 전송

버프스위트로 요청 값을 가로챈 후 다른 사용자의 비밀번호 힌트를 초기화해보자



## 시나리오

### 난이도 : 하

![image-20211027233643532](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027233643532.png)



### Burpsuit

![image-20211027234101709](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027234101709.png)

- intercept on - Any Bugs? 버튼 클릭

### Burpsuit - 1

![image-20211027234322954](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027234322954.png)

- 임의의 사용자로 변경
- secret 값도 변경
- intercept off로 패킷 넘기기



## 확인

### A1. Injection - SQL Injection (Login Form/User)

변경 전

![image-20211027234541308](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027234541308.png)

변경 후

![image-20211027234218276](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027234218276.png)



## 대응방안

### BeeBox

![image-20211027234909751](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027234909751.png)

- 디렉토리 변경
- vi 편집기로 sqli_16.php(해당 웹페이지) 소스 코드 열기

### Beebox - 1

![image-20211027235125478](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027235125478.png)

- Security Level 확인

### BeeBox - 2

![image-20211027235636991](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027235636991.png)

- Level 별 함수 확인
- vi 편집기로 functions_external.php 열어주기

### BeeBox - 3

![image-20211027235920453](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211027235920453.png)

- sqli_check_1 - 난이도 중
  - addslashes : 특수 문자(',",\) 앞에 역슬래시(\) 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
- sqli_check_2 - 난이도 상
  - mysql_real_escape_string : Null, \n, \r, \, ', ", ^Z 문자에 백슬래시를 붙여 SQL Injection 공격 방어