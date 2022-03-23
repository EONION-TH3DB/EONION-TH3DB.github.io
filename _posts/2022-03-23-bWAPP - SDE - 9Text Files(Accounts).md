---
title: "[bWAPP] 6. Sensitive Data Exposure - Text Files (Accounts)"

---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## Text Files (Accounts)

![image-20220323213817015](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323213817015.png)

- 이 시나리오는 평문 패스워드와 SHA-1 패스워드에 대한 취약점을 알아보는 시나리오.

<BR>

### 난이도 : 하

![image-20220323214125112](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214125112.png)

- Download를 눌러보면

<br>

![image-20220323214157729](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214157729.png)

- 다음과 같이 텍스트 파일로서 ID, PW가 저장된 것을 확인할 수 있다.

<BR>

### 난이도 : 중		

![image-20220323214256401](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214256401.png)

- 다음 중 레벨에서는 다음과 같이 해쉬화 되어있는데

<br>

### Kali

![image-20220323214413841](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214413841.png)

- 칼리에서 hash-identifier 로 확인을 해보면 SHA-1 해쉬함수로 암호화 하는 것을 확인할 수 있다.

<BR>

### 난이도 : 상

![image-20220323214514064](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214514064.png)

![image-20220323214607473](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323214607473.png)

- 난이도 상에서는 SHA-256 해쉬 함수로 암호화하고 있고 salt 라는 함수가 추가됐다.
- salt는 해시 함수를 더 안전하게 만드는 역할을 한다.
- 난이도 상에서 만들어진 해시 값은 비밀번호와 salt로 사용된 문자열을 합친 해시 값이어서 해시 함수에 사용된 salt 값을 모르는 경우 비밀번호를 알아낼 수 없다.

<br>

<br>

## 대응방안

스니핑 방지를 위해 데이터는 해시값을 통해 안전하게 전송되어야 한다.

하지만 MD5, SHA-1은 현재 레인보우 테이블에 의해 크랙이 가능하고 짧은 원문일 경우 역해시가 가능하므로 SHA-2 이상을 권장하는 바이다.

SHA-256 해시 함수는 어떤 길이의 값을 입력하더라도 256비트의 고정된 결과값을 출력하기에 일반적으로 입력값이 조금만 변동하여도 출력값이 완전히 달라지기 때문에 출력값을 토대로 입력값을 유추하는 것은 거의 불가능하다.

또한, salt라는 함수를 이용하여 원문에 임의의 문자열을 덧붙여주면 더욱 안정된 패스워드를 구현할 수 있다.