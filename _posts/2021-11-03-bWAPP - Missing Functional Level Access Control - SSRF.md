---
title: "[bWAPP] 7. Missing Functional Level Access Control - Server Side Request Forgery (SSRF)"

---



## Missing Functional Level Access Control

접근 통제와 확인이 서버의 설정이나 관리 측면에서 이루어지지 않을때 발생하는 취약점

대표적으로 파일 다운로드와 업로드 취약점을 이용하여 웹 서버에 접근하는 공격





## Server Side Request Forgery (SSRF)

공격자가 요청을 변조하여 취약한 서버가 내부 망에 악의적인 요청을 보내게 하는 취약점

OWASP 2021 TOP 10의 A.10에 랭크





## 시나리오

### Object

RFI를 사용해 포트를 스캔하고

XXE를 사용하여 내부 망 자원에 접근해보자

![image-20211103213517603](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103213517603.png)



## 1. RFI를 이용한 내부 네트워크 호스트의 포트 스캔

### A7. Remote & Local File Inclusion (RFI/LFI)

![image-20211103215703965](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103215703965.png)

- RFI 취약점이 있는 해당 시나리오로 이동



### Beebox

![image-20211103215907881](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103215907881.png)

- RFI에 사용할 PHP 코드로 var/www/evil/ssrf-1.txt 선택



### ssrf-1.txt

![image-20211103220531199](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103220531199.png)

- ssrf-1.txt는 포트 스캐닝 후 그 결과를 출력하는 PHP 코드
- 스캔하는 포트 배열로 선언 후 포트가 열려있는지 확인
- PHP 코드 실행되면 'U 4r3 0wn3d by MME!!!' 내용 출력



### A7. Remote & Local File Inclusion (RFI/LFI)

![image-20211103220951468](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103220951468.png)

- ssrf-1.txt 소스 코드에서 소켓을 열기위해 ip 변수가 필요하므로 URL에 ip 변수 추가
- 포트를 스캐닝할 웹 서버의 IP주소 입력
- language 변수에 ssrf-1.txt의 경로 입력



### A7. Remote & Local File Inclusion (RFI/LFI)

![image-20211103221202557](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103221202557.png)

- 코드 실행으로 ‘U 4r3 0wn3d by MME!!!’ 메시지 출력
- 확인 누를시 웹 페이지에 포트 스캐닝 결과 출력





## RFI - 대응방안 - Beebox

### Terminal

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104214538713.png" alt="image-20211104214538713" style="zoom:150%;" />

- 디렉토리 변경
- vi 편집기로 해당 시나리오 소스 코드 조회

### rlfi.php

![image-20211104215356539](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104215356539.png)

- Level 확인

### LFI

![image-20211104220754655](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104220754655.png)

- language 변수에 입력할 내용을 배열로 한정하는 화이트 리스트 방식을 사용
- 즉, 안전이 증명된 값만을 취함

### RFI

![image-20211104220938080](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104220938080.png)

- language 변수에 들어오는 입력 값을 생성한 배열로 검증하기 때문에 RFI 공격에 사용한 cmd 변수나 URL은 입력하지 못함





## 2. XXE를 사용하여 내부 네트워크 자원 접근

XML 문서는 엔티티라는 저장 단위로 구성

외부 엔티티를 선언 시 다른 파일의 텍스트 가져올 수 있음



### A7. SSRF

![image-20211103232101702](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103232101702.png)

- Access 클릭



### ssrf-2.txt

![image-20211103232854426](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103232854426.png)

- 1번은 robots.txt를 출력하는 XML 외부 엔티티 공격

- 2번은 heroes.xml를 출력하는 XML 외부 엔티티 공격 << 선택

  - XML은 문서 선언을 위해 DTD를 사용
    - DTD : XML 문서의 구조 및 해당 문서에서 사용할 수 있는 적법한 요소와 속성을 정의, 엔티티를 정의하고 빠른 개발을 위한 내부 DTD를 사용 할 수 있음 
  - DTD 내부에 엔티티 선언하는 구조
  - bWAPP 이라는 엔티티는 heroes.xml 파일을 php://filter/read=convert.base64-encode/resource= 필터를 통해 Base64로 인코딩한 결과를 출력
  - 엔티티 선언 후 선언한 엔티티 참조하기 위해 &bWAPP;를 변수에 입력
  
  

### A1. Stored (XML)

![image-20211103222238156](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103222238156.png)

- XML로 비밀번호 힌트를 초기화하는 시나리오로 이동
- Burp Suite로 Any bugs? 버튼 클릭 시 요청되는 패킷을 잡아보자



### Burp-Suite

![image-20211103233744272](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103233744272.png)

- 1 : proxy - intercept
- 2 : interception on
- 3 : Any bugs? 클릭
- 4 : 우클릭
- 5 : Send to Repeater
- 6 : Repeater



### Burp-Suite

![image-20211104001130595](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104001130595.png)

- body 부분을 ssrf-2.txt 에서 선택한 heroes.xml 부분 복사



### Burp-Suite

![image-20211104002814602](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104002814602.png)

- ssrf-2.txt / heroes.xml 복사 후 Send 버튼 클릭
- Response값에 Base64로 인코딩된 내용 확인



### Burp-Suite

![image-20211104003455818](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211104003455818.png)

- 디코딩 결과 heroes.xml은 superhero 그룹 사용자의 계정 정보를 저장한 파일임을 확인





## XXE - 대응방안

SSRF 대응을 위해서는 요청 값을 필터링하는 정도로는 부족하기 때문에 서버 내부에서 관리적인 대첵 마련

1. 사용하는 포트 외에 다른 포트는 닫기
2. 비정상적인 경로로 접속하거나 유효하지 않은 요청 패킷을 보내는 IP주소는 블랙리스트로 설정하여 접근 차단
3. 내부 망의 방화벽에 시그니처를 설정하여 서버에서 잘못된 요청에 대한 응답을 보내지 않게 방어