---
title: "[bWAPP] 6. Sensitive Data Exposure - BEAST/CRIME/BREACH Attacks"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## BEAST

CBC(암호 블록 체인)의 약점을 이용해 암호화된 세션에서 암호화되지 않은 일반 텍스트를 추출하는 공격 방식.

<BR>

<BR>

## CRIME

HTTP의 압축의 취약점을 이용해 암호화된 HTTP 요청 패킷을 복구하여 쿠키를 훔쳐 세션을 가로채는 공격 기법.

<BR>

<BR>

## BREACH

CRIME exploit 기법을 기반으로 한 공격 기법

이 응답 패킷은 일반적인 HTTP 압축 매커니즘을 사용하여 압축되므로, SSL/TLS 버전에 의존하지 않고 모든 SSL/TLS 버전과 모든 암호 알고리즘에서 작동되어 더 강력한 공격 기법

<BR>

<BR>

## 난이도 : 하

![image-20220323020243239](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323020243239.png)

- Lighttpd 웾 서버는 BEAST, CRIME, BREACH 공격에 취약하다한다.
- 9443 포트에 O-Saft 도구를 사용해서 SSL 연결을 테스트해보라 한다.
- 여러 SSL 스캔도구를 이용해 각 공격기법들에 대해 탐색해보는 시간을 가져보겠다.

<BR>

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

![image-20220323024615277](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024615277.png)

![image-20220323024638388](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024638388.png)

- 비박스 서버의 SSL 연결 정보 확인

<BR>

### +check

![image-20220323024753455](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024753455.png)

![image-20220323024831805](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024831805.png)

![image-20220323024856770](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323024856770.png)

- 암호 체크 부분에서 첫 번째 항목은 암호 이름
- 두 번째 항목은 해당 서버에서 해당 암호를 지원하는지의 여부
- 세 번째 항목은 보안 강도를 의미한다.

<br>

### +cipherall

![image-20220323025044324](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323025044324.png)

- 비박스 SSL 서버에서 사용 가능한 암호 목록을 확인할 수 있다.

<BR>

### sslscan

![image-20220323025511045](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323025511045.png)

- 결과 확인 시 TLS Compression 항목에서 압축 기능이 활성화되어 있다는 것으로 CRIME 공격이 가능함을 알 수 있다.

<br>

### sslyze

![image-20220323025714331](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323025714331.png)

![image-20220323025844273](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323025844273.png)

- 압축 취약점을 포함하여 두 개의 취약점이 있는 것을 확인할 수 있다.

<br>

<br>

## 대응방안

BEAST/CRIME/BREACK와 같은 SSL 취약점을 이용한 공격을 방어하기 위해서는 최신 버전의 SSL을 사용해야하고, 강력한 암호 조합을 사용해야 하며, HTTP 압축 기능을 비활성화 하는 방법이 있다.

<br>

<br>

## Reference

https://blog.naver.com/PostView.naver?blogId=is_king&logNo=221645900327&parentCategoryNo=&categoryNo=89&viewDate=&isShowPopularPosts=false&from=postView