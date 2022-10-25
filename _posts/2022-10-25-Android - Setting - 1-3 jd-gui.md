---
title: "[Android] 1-2 녹스 루팅 탐지 우회 - 환경설정 - jd-gui 세팅"
---

<br>

![image-20221025094948065](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025094948065.png)

# **1. 환경설정**

1-1 녹스 에뮬레이터

- a. 녹스 설치

- b. 녹스 환경 설정

- c.  ADB Shell 연결

1-2 JAVA 설치

- a. JAVA 설치

- b. JAVA 환경변수 설정

**1-3 jd-gui 설치** &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;☚

- **a. jd-gui 란?**
- **b. jd-gui 설치**

1-4 NotePad++ 설치

1-5 테스트 APK 설치

<BR>

<BR>

<BR>

# **1-2 jd-gui**

![image-20221025095045030](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025095045030.png)

<br>

## **a. jd-gui 란?**

JAVA 역 컴파일(**Decompiler**) 도구

JAVA의 특성인 **프로세스 독립적인 언어**는 역 컴파일이 가능한 구조

JAVA의 소스파일은 **.class** 파일로 컴파일되어야 **실행** 가능

**.class** 파일만 있다면 본래의 해당 클래스 파일의 **기능만 추출 가능**하지만, **jd-gui**를 통해 **.class**파일로 자바 원본 소스를 **다시 복원 가능**하다.(95%정도)

<br>

### **디컴파일**

.dex → .class → .java

잃어버린 코드, 컴퓨터 보안, 오류 검출 정정 등의 과정에서 사용

난독화가 진행될 수록 디컴파일된 소스 이해하기 어렵지만, 최근 추세로는 난독화에 대한 요구 줄어드는 추세(오픈 소스의 범용화, 빠르고 복잡해진 사업)

![image-20221025102652293](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025102652293.png)

- 난독화된 소스들은 모든 변수와 클래스가 의미 없는 문자열로 치환되기에 a, b, c 와 같은 알파벳 class 파일, 함수들이 나열된다.

<br>

### **컴파일**

.java → .class → .dex

디컴파일한 소스를 컴파일할 경우 라이브러리 참조 필수

해당 class를 java로 100% **디컴파일** 했더라도 **다시 컴파일**시 참조한 **라이브러리**까지 같이 컴파일해야 에러가 나지 않는다.

<br>
<br>

## **b. jd-gui 설치**

[http://java-decompiler.github.io/](http://java-decompiler.github.io/)

![image-20221025104339191](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025104339191.png)

Installer의 자동화된 환경설정 파일의 추가가 오히려 충돌을 일으킬 수 있기 때문에 필자는 zip 파일을 선호한다.
