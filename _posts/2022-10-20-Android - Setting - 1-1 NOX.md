---
title: "[Android] 1-1 녹스 루팅 탐지 우회 - 환경설정 - NOX 세팅"
---

<br>

![image-20221021140022390](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021140022390.png)

# **1. 환경설정**

1-1 녹스 에뮬레이터

- a. 녹스 설치

- b. 녹스 환경 설정

- c.  ADB Shell 연결

1-2 JAVA 설치

- a. JAVA 설치

- b. JAVA 환경변수 설정

1-3 jd-gui 설치

1-4 NotePad++ 설치

1-5 테스트 APK 설치

<BR>

<BR>

<BR>

# **1-1 NOX**

![image-20221021144408270](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021144408270.png)

<br>

## 정의

**안드로이드** 앱을 PC에서 실행할 수 있게 해주는 **에뮬레이터**

우리는 **안드로이드** **모바일 진단**을 실행하기에 앞서 실 기기가 아닌 **녹스**로 진행하겠다.

- 실 기기보다 루팅에 있어 편리하기에 선택했다

<BR>

<BR>

## **a. NOX 설치**

[https://kr.bignox.com/](https://kr.bignox.com/ ) 접속해서 **다운로드**하고 다음과 같이 진행해주자

![image-20221020143959878](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020143959878.png)

![image-20221020144132288](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020144132288.png)

![image-20221020144329009](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020144329009.png)

<br>

<BR>

## **b. NOX 환경 설정**

설치 후 안드로이드 기기의 **루팅** 상태와 **동일**하게 **녹스 루팅** 환경을 설정해주자

![image-20221020144715942](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020144715942.png)

<BR>

먼저, **해상도 설정**을 모바일 기기 보듯이 설정해주자

- 해상도 설정
  - 스마트폰
  - 540x960

![image-20221020151449976](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020151449976.png)

<BR>

다음으로. **일반 설정**으로 이동해주자

- 일반 설정
  - ROOT 켜기 체크
  - 설정저장 후 녹스 재부팅

![image-20221020145410439](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020145410439.png)

<BR>

<BR>

## c. ADB Shell 연결

### ADB란?

**안드로이드** 장치와 **통신**하여 디버깅 등의 작업을 진행하는 **Command Line tool** 이다

안드로이드 SDK에도 포함되어 있으며 **앱 설치**, 파일 업로드 및 다운로드, 시스템 log 출력, **shell 및 디바이스 접속** 등이 가능하다.

녹스 설치 시 **nox_adb가 설치**되어 사용 가능하다.

따라서, 우리는 **녹스 ADB Shell**로 녹스라는 안드로이드 에뮬레이터와 **통신**하여 **안드로이드 진단**을 시작하겠다.

<BR>

[**윈도우**] 키 누르고 **NOX** 타이핑

**[파일 위치 열기]** 클릭

![image-20221020152118374](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020152118374.png)

<BR>

[**Nox** 아이콘] 우클릭 후 **속성** 클릭

![image-20221020152332063](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020152332063.png)

<br>

**[파일 위치 열기**] 클릭

![image-20221020152454556](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020152454556.png)

<br>

nox_adb.exe 파일 **경로** 확인

- 현재 경로 명령프롬프트로 nox_adb를 연결해도 되지만 편리하게 **환경변수**까지 설정해주자

![image-20221020152636261](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020152636261.png)

<br>

[윈도우 키 + R] - [sysdm.cpl 입력] - [고급] - [환경 변수(N)] 순으로 이동 후

[시스템 변수(S)] - [편집(I)] 으로 이동

![image-20221020153003509](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020153003509.png)

<br>

[새로 만들기] - [nox_adb 설치되어 있던 경로 입력] - [확인]

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTMLdbfc96e.PNG)

<br>

[윈도우 키 + R] - [CMD] 명령프롬프트 창 띄우기

**nox_adb shell** 입력 후 **연결 확인**

![image-20221020153646493](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221020153646493.png)
