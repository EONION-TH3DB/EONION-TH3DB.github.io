---
title: "[Android] 1-1. 안드로이드 구조"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

<br>

# 안드로이드 구조

- 애플리케이션
- 프레임워크
- 런타임
- 네이티브 라이브러리
- 커널

<br><br>

## 애플리케이션 계층

![image-20221123193736427](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123193736427.png)

Androud 구조 스택의 **최상위** 계층으로써, 사용자와의 **상호작용**

배포하여 설치하는 **APK**가 여기에 속함

<br>

### 시스템 애플리케이션

**/system** 디렉터리에 존재하며 사용자가 제거/변경 **불가능**

**메세지** 애플리케이션, **기본** 애플리케이션 등이 해당

<br>

### 사용자 애플리케이션

**/data** 디렉터리에 존재하며 사용자가 제거/변경 **가능**

각각의 애플리케이션은 **샌드박스**에 설치되어 영향을 주거나 **데이터 통신** 불가

각 애플리케이션의 권한을 가진 **리소스**에만 접근 **가능**

**인스타그램**과 같은 일반 애플리케이션 등이 해당

<br><br>

## 프렘임워크 계층

![image-20221123193800891](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123193800891.png)

이 계층부터 **JAVA**로 작성하며 기본 라이브러리와 안드로이드 런타임을 **추상화**한 계층

JAVA **API** 가 존재하며, 안드로이드 OS의 기능은 해당 **API**를 통해 접근

<br>

### 주요 API

- Active Manager : 애플리케이션 내 **Activity**들 관리
- Content Manager : 애플리케이션 간 **데이터 공유** 관리
- Telephony Manager : **음성 통화** 관리
- Resource Manager : **위치 정보** 관리
- Notification Manager : 애플리케이션에서 사용하는 **리소스**들 관리

<br><br>

## 런타임 계층(ART, 구 Dalvik)

![image-20221123193917530](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123193917530.png)

안드로이드 전용 **JVM**(Java Virtual Machine)

JAVA는 컴파일 시 CPU나 플랫폼 환경에 맞는 언어가 아닌 **바이트 코드**로 컴파일된다.

JAVA는 바이트 코드 하나로 **여러가지** 플랫폼에서 작동할 수 있게 하는 것이 목표이고,

플랫폼에 맞는 가상 머신만 있다면 하나의 **실행** 파일로 **각종** 장치에 쓸 수 있기 때문이다.

따라서, 바이트 코드를 실행하기 위해서는 **JVM**이 필요하다.

이 **JVM**은 자바 바이트 코드를 **DEX** 포맷으로 변환(컴파일)하여 **패키징** 후 실행

<br>

### Dalvik - JIT

JIT(Just In Time) : 앱이 실행되는 순간 **자주 사용**되는 코드를 컴파일

- 실행시간 **느림**
- JVM 라이선스 문제로 Dalvik VM을 따로 개발

<br>

### ART - AOT

AOT(Ahead On Time) : 앱 설치 시 **모든** 코드를 컴파일

- 설치속도는 느리지만 실행속도는 **빠름**, 배터리 소모 큼
- Android **5.0** 부터 사용
- 현재는 Dalvik과 ART를 **결합**한 JVM을 사용중이다.

<br><br>

## 네이티브 라이브러리 계층

![image-20221123193826426](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123193826426.png)

Android는 상대적으로 작은 **주기억장치**와 저전력의 **CPU**에서 동작하기에 이러한 기기에 최적화된 **원시코드**로 컴파일 된다.

**libc**, **libm** 과 같은 네이티브 라이브러리는 특히 메모리 소비가 적기에 안드로이드에 **적합**한데,

이러한, 네이티브 라이브러리를 **제공**하는 기능을 하는게 네이티브 라이브러리 계층이다.

**C/C++**로 작성되어 있다.

<br>

### 라이브러리

- SGL : **2D** 그래픽 담당
- Open GL ES : **2D/3D** 그래픽 담당
- Free Type : **폰트** 렌더링
- Seface : **Surface** 결합을 담당
- Webkit : 웹 **브라우저** 엔진
- SQLite : **경량 DB** 엔진
- SSL : Secure Socket Layer **프로토콜**
- Media : **코덱** 지원
- libc : 시스템 **C** 라이브러리

<br><br>

## 커널 계층

![image-20221123193939748](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123193939748.png)

Android 계층 **최하단** 위치하며, 시스템 전체의 **중심** 역할

사용자나 개발자가 이쪽 계층에서 작업하거나, **상호 작용**할 일은 **없다**.

**리눅스 커널**을 기반으로 제작된 OS로써 안드로이드 버전과 함께 커널의 **버전**도 업데이트 되고 있다.

<br>

### 기능

- **하드웨어** 추상화
- **보안** 설정
- **메모리** 관리
- **전원** 관리
- **네트워크** 시스템 관리
- **장치** 드라이버 관리
- H/W, 네트워크, 파일 시스템 **접근**

<br>

**커널**과 **라이브러리**를 제외하고는 JAVA로 작성되어 가상머신상에서 동작한다.