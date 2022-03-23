---
title: "[bWAPP] 6. Sensitive Data Exposure - Host Header Attack (Reset Poisoning)"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## Host Header Attack (Reset Poisoning)

![image-20220323044555781](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323044555781.png)

- email을 입력하면 reset을 바꿀수 있게 해주는 시나리오

<br>

![image-20220323044948187](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323044948187.png)

- 실제로는 전송되질 않아 직접 출력해보자

<br>

### bWAPP

![image-20220323045032167](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323045032167.png)

![image-20220323045237776](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323045237776.png)

- $content 변수 확인하고

<br>

![image-20220323045312742](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323045312742.png)

- 출력 부분에 content 변수도 출력되게끔 해주자.

<br>

![image-20220323045544675](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323045544675.png)

- 다음과 같이 출력된다.
- 이 때 URL 주소가 하나 나오는데 저 부분을 우리가 만든 임의의 페이지 링크로 바꿔서 희생자의 이메일 계정으로 보내, 희상자가 이 링크를 클릭하도록 유도해보자.

<BR>

### BurpSuite

![image-20220323045916842](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323045916842.png)

![image-20220323050038751](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323050038751.png)

- Reset 버튼을 눌러 패킷을 캡쳐해주자
- Host 부분을 다음과 같이 바꿔서 진행해보자

<br>

![image-20220323050117048](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323050117048.png)

- 다음과 같이 변경된것을 확인할 수 있다.
- 이를 이용해 특정 계정의 secret을 변경할 수 있는 Host Header Attack을 진행해보자
  - 비박스 IP [희생자 이용 웹 서버] : 172.30.1.16
  - 칼리 IP [공격자 사용 웹 서버] : 172.30.1.37

<br>

### Kali

![image-20220323050450514](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323050450514.png)

- 아파치 서버 구동해주고
- 디렉토리 생성후 사용자의 reset_code를 탈취하여 저장 등 파일입출력을 해야하기 때문에 bwapp 디렉토리의 소유자를 바꿔준다.

<br>

![image-20220323050956723](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323050956723.png)

- secret_change.php 이다.
- 또한, 최종적으로 디렉토리는 /var/www/html/bWAPP이 되어야 한다.
- 보기는 칼리에 비박스를 설치해놔서 bWAPP 디렉토리가 존재하기 때문에 secret_change.php 파일을 생성할 곳이 없다.

![image-20220323050931590](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323050931590.png)

- vi 편집기를 통해 다음과 같이 작성해준다.
- GET으로 받은 email, reset_code 변수를 각각 변수에 저장한 뒤에, token.txt 파일에 해당 변수를 저장하는 코드이다.

<br>

### BurpSuite

![image-20220323051209336](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323051209336.png)

- Host를 공격자[Kali]로 변조한 뒤, 패킷을 넘겨주자.

<br>

![image-20220323051253853](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323051253853.png)

- 성공적으로 변경된것을 확인할 수 있고 희생자가 저 사이트로 접속하게 되면 secret_change.php에 입력한 내용을 보게 될 것이고 희생자의 reset_code가 칼리의 token.txt에 자동 생성될것이다.

<br>

![image-20220323052016981](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323052016981.png)

![image-20220323052036603](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323052036603.png)

- 다음과 같이 reset_code를 확인할 수 있다.

<br>

<br>

## 대응방안

![image-20220323053257162](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323053257162.png)

- vi 편집기를 통해 hostheader_2.php 소스코드를 살펴보면
- Medium과 High 레벨에서 $server 변수 즉 Host 주소를 itsercgames.com으로 고정하고 있기에
- 앞서 진행헀던 공격자 서버로 호스트 헤더 변조는 되질 않는다.

<br>

<br>

## Reference

https://m.blog.naver.com/is_king/221651178279