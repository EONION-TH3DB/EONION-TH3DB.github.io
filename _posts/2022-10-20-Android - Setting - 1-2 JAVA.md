---
title: "[Android] 1-2 녹스 루팅 탐지 우회 - 환경설정 - JAVA 세팅"
---

<br>

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTML1294419f.PNG)

# **1. 환경설정**

1-1 녹스 에뮬레이터

- a. 녹스 설치

- b. 녹스 환경 설정

- c.  ADB Shell 연결

**1-2 JAVA 설치**

- **a. JAVA 설치**

- **b. JAVA 환경변수 설정**

1-3 jd-gui 설치

1-4 NotePad++ 설치

1-5 테스트 APK 설치

<BR>

<BR>

<BR>

# **1-2 JAVA**

![image-20221021143725457](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021143725457.png)

<BR>

## **JAVA란?**

Java는 **1995년 Sun Microsystem**에서 처음 릴리스된 프로그래밍 언어 및 컴퓨팅 플랫폼이다.

**가전제품에 들어갈 다양한 프로세스**에서 동작하는 독립적인 언어를 만드는 과정에서 Java는 탄생했다.

Java는 많은 서비스 및 애플리케이션이 구축된 **안정적인 플랫폼**을 제공한다,

데스크톱 Java를 설치하지 않거나 버전이 맞지 않는다면 애플리케이션 및 일부 웹 사이트까지 **작동하지 않는 경우**가 있다.

Java는 버전에 따라 분석할 수 있는 **데이터의 범위가 달라**지고, 개발자나 비즈니스 담장자가 부르는 **버전의 명칭**까지 **다르기**까지 하다.

이유는, Java의 제품 버전과 개발자 버전의 **규칙이 다르기** 때문이다.

따라서 목적에 맞는 **세팅환경(도구 버전 정보)**을 맞춰주는게 중요하다.

<br>

<BR>

## a. Java 설치

- Java Version : 1.8.0_161
- 다운로드 주소 : [https://www.oracle.com/kr/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/kr/java/technologies/javase/javase8-archive-downloads.html)
- 자신 **운영체제에 맞는 환경**으로 설치해주자

![image-20221021142413256](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021142413256.png)

<br>

<BR>

## **b. Java 환경변수 설정**

[윈도우 키 + R] - [sysdm.cpl 입력] - [고급] - [환경 변수(N)] 순으로 이동 후

[시스템 변수(S)] - [새로 만들기(W)]

![image-20221021142342383](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021142342383.png)

<BR>

- 변수 이름 : JAVA_HOME
- 변수 값 : JDK 설치 폴더 경로

![image-20221021143010904](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021143010904.png)

<BR>

Path 변수 찾은 뒤 [편집] 클릭

![image-20221021143137018](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021143137018.png)

<br>
[새로 만들기] 클릭 후

%JAVA_HOME%bin 추가 후 [확인]

![image-20221021143334999](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021143334999.png)

<br>

확인

- 명령어 : java -version

![image-20221021143616075](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221021143616075.png)