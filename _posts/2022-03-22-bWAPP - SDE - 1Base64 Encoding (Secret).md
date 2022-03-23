---
title: "[bWAPP] 6. Sensitive Data Exposure - Base64 Encoding (Secret)"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## Base64 Encoding

8bit 이진 데이터를 일종의 64진수 문자셋으로 변환하는 인코딩 기법

Base64 인코딩은 이진데이터를 6bit씩 표현하는데 다른 이진 데이터 변환 규칙과 관계 없이 아스키코드만으로 데이터를 인코딩하므로 플랫폼 제한 없이 사용할 수 있다.

이진 데이터가 6bit 씩 나누어지지 않을 때는 패딩 문자를 자동으로 붙여 6bit을 채우는데 8bit 인코딩하는 기법보다 데이터가 커지지만, 다양한 플랫폼에서 전송한 데이터가 인코딩 기법이 달라 깨지는 경우가 없기 때문에 Base64 인코딩 기법을 주로 사용한다.

<BR>

<BR>

## 난이도 : 하

![image-20220323015337274](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323015337274.png)

- secret 쿠키 값을 확인해보자

<br>

![image-20220323015411065](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323015411065.png)

- Base64로 인코딩되어 있는 것 확인
- 디코딩해보자

<br>

![image-20220323015659406](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323015659406.png)

<BR>

<BR>

## 대응방안

![image-20220323052517931](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323052517931.png)

- vi 편집기로 insecure_crypt_storage_3.php 소스코드를 살펴보면
- Medium과 High에서는 SHA-1 해시 함수를 사용한다.
- 해시 알고리즘에서 MD5, SHA-1 알고리즘은 복잡한 문자열의 경우 역해시가 불가능하지만, 결함이 있기 때문에 단순한 문자열이 원문일 경우에는 해시 값으로 원문을 찾을 수 있다.
- 그렇기에 더 강력한 보안을 위해서는 SHA-2 이상을 권장한다.
