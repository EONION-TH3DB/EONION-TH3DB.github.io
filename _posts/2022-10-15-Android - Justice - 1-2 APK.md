---
title: "[Android] 1-2. APK 구조"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

<br>

# APK 구조

- APK란?
- APK 파일 구조
- 컴파일 / 디컴파일 / 패키징 / 리패키징
- 애플리케이션 진단 도구

<br><br>

## APK란?

![image-20221124152114161](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221124152114161.png)

 **A**ndroid **A**pplication **P**a**k**age

안드로이드 프로그램 **형태**로 배포되는 형식의 파일

JAR를 **확장**한 포맷이먀, JAR 파일은 **ZIP** 압축 파일 포맷을 사용한다.

<br>

### APK 설치 경로

`/data/app/<APK명>`

`/data/data/<APK명>`

<BR><br>

## APK 파일 구조

![image-20221124153240710](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221124153240710.png)

### META-INF

Signature File

공개키를 담은 **인증서** 등이 존재

<br>

### res

애플리케이션이 실행하는데에 필요한 **리소스**(레이아웃, 문자열 등)가 모여있는 **디렉토리**

<br>

### AndroidManifest.xml

패키지 정보, 권한, 앱을 구성하는 컴포넌트 및 패키지 정보, 권한과 같은 **앱의 정보**가 저장되는 파일

주요 **컴포넌트**

- Activity : 일반적으로 **하나의 뷰**를 말하며, 해당 Activity에 대한 **속성**을 정의한다.
- Service : **백그라운드**에서 실행되는 서비스
- Broadcase Receiver : 안드로이드 **내부 이벤트 핸들링**을 위한 컴포넌트
  - ex) 배터리 부족, 문자 수신
- Content Provider : 애플리케이션 간 **데이터 공유**를 위한 컴포넌트

<br>

### Classes.dex

.class 파일을 **바이트 코드**로 변환(컴파일)시킨 소스 파일

애플리케이션 **실행**을 위한 코드 존재

<br>

### Resources.arsc

컴파일된 **리소스**(문자열, 스타일 등)와 **res**의 정보 존재

<br><br>

## 컴파일 / 디컴파일 / 패키징 / 리패키징

### APK 컴파일

**JAVA** -JavaCompile-> **CLASS** -DexCompile-> **.Dex**

<br>

### APK 디컴파일

**JAVA** <-JavaDecompile- **CLASS** <-Dex2jar- **.Dex**

<br>

### 안드로이드 앱 패키징

![image-20221124164615143](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221124164615143.png)

일반적으로 프로그래밍 언어들은 컴파일 과정을 거쳐 기계가 인식할 수 있는 **바이너리 파일**로 변환(컴파일)되는데,

안드로이드는 이 과정에서 프로그램을 묶어주는 **패키징**과 **코드 사인** 작업이 추가된다.

전 게시물에서 설명했다싶이, JAVA는 **바이트 코드**로 변환(컴파일)된다.

[https://eonion-th3db.github.io/Android-Justice-1-1-Android/](https://eonion-th3db.github.io/Android-Justice-1-1-Android/) ← 전 게시물

<br>

### 안드로이드 앱 리패키징

![image-20221124164529909](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221124164529909.png)

앱의 제작과정을 **거꾸로** 하여 앱의 최초 형태인 소스 코드로 변환한 후 소스 코드를 **수정**하거나 다른 코드를 삽입하고 앱을 **다시 제작**하는 일련의 과정

<br><br>

## 애플리케이션 진단 도구

### ADB shell

녹스 혹은 단말과 통신을 위한 명령 수행 콘솔

패키지 설치, 삭제, 쉘 명령 등 수행

<br>

### Apktool

APK 파일을 디컴파일 및 리빌드하기 위한 도구

<br>

### Dex2jar

Dex 파일을 자바 클래스(CLASS) 파일로 변환(컴파일)

<br>

### jd-gui

Java Decompiler 도구

<br>

### Keytool

APK 파일 서명하기 위한 키 생성 도구

<br>

### Jarsigner

리빌드된 APK 파일에 코드 서명을 위한 도구

<br><br>

앞으로 위 도구들을 사용해서 **루팅 탐지 우회**, **메모리 내 중요정보** 탈취를 진행해보자
