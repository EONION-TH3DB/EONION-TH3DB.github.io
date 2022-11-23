---
title: "[Android] 프록시 설정(NOX) - 플러터(Flutter)"
---

<br>

![image-20221123155150430](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123155150430.png)

# **Flutter**

- Flutter란?
- Android 내 Flutter 프록시 설정

<br><br>

## Flutter란?

![image-20221123155923215](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123155923215.png)

안드로이드와 iOS 모바일 애플리케이션을 **하나의 코드로 개발**(크로스 플랫폼)할 수 있는 UI 프레임워크

**다트(Dart)** 언어 기반의 구글에서 개발한 프레임워크로 구글에서 개발하는 퓨시아 오픈 소스 운영체제의 메인 개발환경

### 장점

- **빠른 개발** : 핫 리로드 기능으로 쉽고 빠르게 UI 빌드 및 기능 추가, 디버그 가능
- 표현력있는 **UI** : 내장된 메테리얼 디자인, 쿠퍼티노 위젯, API가 사용자 친화적이어서 앱 개발에 수월
- **고성능**, 플랫폼 밀착적인 퍼포먼스

### 단점

- 다트언어로써 파이썬과 자바스크립트의 자유로운 자료구조 사용에 비해 제네릭 적용으로 **개발에 있어 답답함**
- **적은** 라이브러리
- **적은** 자료 정보

<br><br>

## Android 내 프록시 설정

![image-20221123160916824](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123160916824.png)

플러터 환경의 모바일 애플리케이션에서 Burp Suite(프록시) 연결 시 **패킷**(HTTP/HTTPS)이 잡히지 않는다.

일반적으로 **Burp 인증서**를 단말기 또는 브라우저에 **설치**해서 HTTP 통신을 수행하는데,

Flutter 애플리케이션 경우 **CA 저장소를 사용하지 않는 Dart 언어 특성**상 패킷이 잡히지 않는다.

Flutter는 애플리케이션 **자체에 포함된 CA 목록을 사용**하기에 일반적인 Proxy 툴에서는 인식하지 못하는데,

다음 **두가지** 방법으로 **우회**가 가능하다.

<br>

<br>

## 애플리케이션 내 프록시 서버 지정

[https://api.dart.dev/stable/2.4.0/dart-io/HttpClient/findProxyFromEnvironment.html](https://api.dart.dev/stable/2.4.0/dart-io/HttpClient/findProxyFromEnvironment.html)

<br><br>

## iptables 이용한 패킷 리다이렉션

![image-20221123161928642](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123161928642.png)

`nox_adb shell // 녹스 adb shell에 접속`

`su // 루트권한 부여`

`iptables -t nat -L // 현재 설정내역 확인`

<br>

![image-20221123162312668](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123162312668.png)

`iptables -t nat -A OUTPUT -p tcp -j DNAT --to-destination <로컬PC ip주소>:<포트번호> // Proxy 설정`

<br>

![image-20221123163029369](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123163029369.png)

iptables 설정을 해줬는데도 패킷이 잡히지 않는다면 다음 설정을 체크해주자

<br>

<br>

## iptables 설정 삭제

![image-20221123162703523](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123162703523.png)

`iptables -t nat -D OUTPUT 1 // 삭제`