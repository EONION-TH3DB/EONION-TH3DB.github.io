---
title: "[bWAPP] 6. Sensitive Data Exposure - Clear Text HTTP (Credentials)"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## Clear Text HTTP

![image-20220323030435014](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323030435014.png)

- 계정 및 비밀번호와 같은 개인정보를 처리하는 페이지에서 네트워크 프로토콜 보안은 필수다.
- SSL을 사용하지 않는 통신에서 평문 데이터가 ARP 스푸핑 공격을 통해 노출되는 것을 확인해보자.

<BR>

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323220050901.png" alt="image-20220323220050901" style="zoom:150%;" />

- 클라이언트와 서버간의 전송하는 과정에서 클라이언트는 서버에게 요청을 보내고 서버는 그에 맞는 응답을 보내게 되는데
- 이 때 공격자가 ARP 스푸핑 공격을 하게 되면 클라이언트에게 공격자의 맥주소 bb가 서버의 맥주소로 나타나고
- 서버는 공격자의 맥주소 bb가 클라이언트의 맥주소인 마냥 나타나게 된다.

<BR>

### Kali

![image-20220323030751537](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323030751537.png)

![image-20220323030805674](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323030805674.png)

- 스니핑 도구인 Ettercap 프로그램을 -G 옵션을 선텍하여 그래픽 모드로 실행해주자.

<br>

![image-20220323032019441](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323032019441.png)

- 순번대로 눌러주고 host list로 들어간 후 scan for hosts를 눌러 호스트들을 검색해주자.

<br>

![image-20220323034605017](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323034605017.png)

- Target 1 에는 클라이언트 주소(Window)
- Taget 2 에는 서버(bWAPP) 주소를 연결해주자.

<br>

### Window

![image-20220323032423982](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323032423982.png)

![image-20220323034701456](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323034701456.png)

- arp -a 를 통해 살펴보면
- 1번이 공격자 - Kali 
- 2번이 서버 - bWAPP
- 이므로 Ettercap을 통해 ARP 스푸핑을 진행했을때 서버의 맥주소가 칼리의 맥주소로 바뀌는지 확인하면 된다.

<BR>

### Ettercap

![image-20220323032636412](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323032636412.png)

![image-20220323032650416](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323032650416.png)

![image-20220323034738175](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323034738175.png)

- 다음과 같이 서버의 맥주소가 칼리의 맥주소로 동일하게 변경된것을 확인하여 성공적으로 스푸핑을 진행했다.

<br>

![image-20220323034818693](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323034818693.png)

![image-20220323034835871](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323034835871.png)

- 또한, 로그인시 로그인 정보와 전달되는 값들이 평문으로 나타난다.

<br>

<br>

## 대응방안

![image-20220323052903613](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323052903613.png)

- vi 편집기를 통해 insuff_transp_layer_protect_1.php 소스 코드를 열어주면
- Medium과 High 레벨에서는 url 변수에 https로 입력하는 것을 볼 수 있다.
- 웹 페이지에서 또한 난이도 중과 상에서 insuff_transp_layer_protect_1.php 페이지로 들어가게 되면 HTTPS로 연결된다.
- 즉, 중요한 정보를 전송하는 경우 HTTPS 포로토콜을 사용해야 한다.
- HTTPS 프로토콜로 중요 정보를 전송하려면 서버에 SSL 인증서를 설치하여 암호화 설정을 해야하는데
- SSL 인증서 발급은 공인 CA를 통해 인증서를 구입해야 한다.
