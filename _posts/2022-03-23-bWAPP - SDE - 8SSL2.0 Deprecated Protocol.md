---
title: "[bWAPP] 6. Sensitive Data Exposure - SSL 2.0 Deprecated Protocol"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## SSL 2.0 Deprecated Protocol

Deprecated : 중요도가 떨어져 더 이상 사용되지 않고 앞으로는 사라지게 될 의미의 단어

![image-20220323211951829](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323211951829.png)

- 웹 서버의 암호 체계는 취약하다고 알려진 오래된 사라질 포로토콜을 사용한다고 한다.
- SSL2.0 을 비활성화 시키고 대신에 SSL3.0 또는 TLS 1.x 버전을 사용하라고 한다.
- 또한, O-Saft 툴을 이용해 SSL 연결을 테스트 하라고 한다.
- 즉, 이 시나리오는 bWAPP 에서 동작중인 SSLv2 취약점을 'O-Saft' 도구를 이용해 스캔하라는 내용이다.

<br>

### O-Saft

![image-20220323024138361](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024138361.png)

- 다음과 같이 디렉토리를 설정해서 O-Saft를 사용해보자

o-saft 정보

- +list : 이 도구에서 지원하는 모든 암호 목록을 출력
- +version : 버전 정보 출력

SSL 연결에 대한 세부 사항

- +info : SSL 연결에서 중요한 정보를 출력
- +http : HTTP 검사
- +check : 보안 문제와 관련한 SSL 연결을 점검

대상 서버에서 제공하는 암호 테스트	

- +cipherall : 해당 SSL에서 사용 가능한 암호 확인

<BR>

### +info

![image-20220323212938911](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323212938911.png)

![image-20220323213100457](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213100457.png)

- 비박스 서버의 SSL 연결 정보 확인

<BR>

### +check

![image-20220323213221690](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213221690.png)

![image-20220323213240649](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213240649.png)

![image-20220323213312202](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213312202.png)

- 암호 체크 부분에서 첫 번째 항목은 암호 이름
- 두 번쨰 항목은 해당 서버에서 해당 암호 지원 여부
- 세 번째 항목은 보안 강도를 의미한다.

<br>

### +cipherall

![image-20220323213422135](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213422135.png)

- 비박스 SSL 서버에서 사용 가능한 암호 목록을 확인할 수 있다.

<BR>

<BR>

## 대응방안

SSL 취약점을 이용한 공격을 방어하기 위해서는 최신 버전의 SSL을 사용해야하고, 강력한 암호 조합을 사용해야하며, HTTP 압축 기능을 비활성화 하는 방법이 있다.
