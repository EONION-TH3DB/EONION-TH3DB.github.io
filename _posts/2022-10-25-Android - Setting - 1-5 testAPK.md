---
title: "[Android] 1-5 녹스 루팅 탐지 우회 - 환경설정 - TestAPK 세팅"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

# **1. 환경설정**

1-1 녹스 에뮬레이터

- a. 녹스 설치

- b. 녹스 환경 설정

- c.  ADB Shell 연결

1-2 JAVA 설치

- a. JAVA 설치

- b. JAVA 환경변수 설정

1-3 jd-gui 설치

- a. jd-gui 란?
- b. jd-gui 설치

1-4 NotePad++ 설치

- a. NotePad++ 란?
- b. NotePad++ 설치

**1-5 테스트 APK 설치** &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;☚

- **APK 란?**
- **테스트 APK 설치**

<BR>

<BR>

<BR>

# **1-5 APK**

![image-20221025131556348](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025131556348.png)

<br>

## **a. APK 란?**

**Android Application Pakage**의 약자로 안드로이드에서 프로그램 형태로 배포되는 형식의 파일

소프트웨어 설치에 사용되는 **WindowsOS**의 **.exe**파일과 같다.

APK 파일에는 애플리케이션의 **모든 데이터**가 포함

플레이스토어에서 애플리케이션 설치시 Android가 **백그라운드**에서 앱 설치 프로세스를 처리

**JAR**를 **확장한 포맷**이며, **JAR**파일은 **ZIP** 압축 파일 포맷을 사용함

<br>

### **APK 파일 구조**

![image-20221025132850874](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025132850874.png)

#### **Folder**

- META-INF

  **Signature File**

  **공개키**를 담은 인증서 등이 존재

- res

  &emsp;&emsp;&emsp; 애플리케이션이 사용하는 **리소스**(레이아웃, 문자열 등) 존재

#### **File**

- AndroidManifest.xml

  &emsp;&emsp;&emsp; 패키지 **정보**, **권한**, 앱의 **구성 요소(**Activity 등) 정보를 포함한 파일

  &emsp;&emsp;&emsp; 안드로이드 애플리케이션 구성하는 **컴포넌트**가 포함된 파일

  &emsp;&emsp;&emsp; &emsp;&emsp;&emsp; 애플리케이션 구성하는 컴포넌트에 대한 **클래스명** 및 **intent-filter**를 정의

  &emsp;&emsp;&emsp; 컴포넌트

  &emsp;&emsp;&emsp; &emsp;&emsp;&emsp; Activity : 일반적으로 하나의 **뷰**, 해당 Activity에 대한 **속성**을 정의

  &emsp;&emsp;&emsp; &emsp;&emsp;&emsp; Service : **백그라운드**에서 실행되는 서비스

  &emsp;&emsp;&emsp; &emsp;&emsp;&emsp; Broadcast Receiver : 안드로이드 내부 **이벤트** **핸들링**을 위한 컴포넌트 / ex) 문자 수신, 배터리 부족

  &emsp;&emsp;&emsp; &emsp;&emsp;&emsp; Content Provider : 애플리케이션간 **데이터 공유**를 위한 컴포넌트

- Classes.dex

  &emsp;&emsp;&emsp; 애플리케이션 실행을 위한 코드가 존재

- Resources.arsc

  &emsp;&emsp;&emsp; 컴파일된 리소스(문자열, 스타일 등) 존재

<br>

### **APK 파일 경로**

![image-20221025132117266](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025132117266.png)

1-1강에서 확인했던 nox_adb shell 연결 한 후 cd 명령어로 디렉터리를 변경하자

- /data/app/<PakageName>
- /data/data/<PakageName>

<br>

<br>

## b. TestAPK 설치

![image-20221025134212465](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025134212465.png)

[https://github.com/OWASP/owasp-mastg/blob/master/Crackmes/Android/Level_01/UnCrackable-Level1.apk](https://github.com/OWASP/owasp-mastg/blob/master/Crackmes/Android/Level_01/UnCrackable-Level1.apk)

위 사이트로 접속해 OWASP에서 배포한 테스트 APK를 다운로드 하자.

<BR>

![image-20221025134707990](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025134707990.png)

nox_adb install [apk 설치경로 및 파일명]

사실 apk 파일 더블클릭만해도 설치된다.

<br>

![image-20221025134827681](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025134827681.png)

![image-20221025134908259](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025134908259.png)

앱 설치 후 실행 시 루팅된 단말기로 탐지되었다는 문구와 함께 앱 사용 불가

&emsp;&emsp;&emsp; 1-1강에서 루트를 켰기에 현재 녹스 에뮬레이터는 **루팅된 단말기**이다.

<br>

진단하기에 앞서 **세팅**을 모두 마쳤다.

**2강**으로 넘어가 루팅된 단말기로써 **앱을 사용못하는 상황**을 **우회**해보도록 하겠다.