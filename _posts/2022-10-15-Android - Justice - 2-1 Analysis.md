---
title: "[Android] 2. APK 정적/동적 분석"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

<br>

# APK 분석

1. APK 정적 분석
2. APK 동적 분석

<br><br>

# 1. APK 정적 분석

![image-20221125133633934](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125133633934.png)

**소스코드** 상에서 결함을 찾아내는 분석 기법

**Smali**, **AndroidManifest**.xml 등 **소스코드** 단에서 취약점 발생 가능성 확인하는 기법

<BR>

## OS 변조 탐지 기능 적용 여부

OS가 **변조**(루팅, 탈옥)된 단말 이용 시 보안 위협이 증대됨이 따라, OS **변조** 시 서비스 **이용가능** 여부를 점검

<br>

### su 명령어 확인

- 루트 명령어(**su**) 실행 여부로 루팅 및 탈옥 확인

### 파일 이름 기반 확인

- 루팅 시 **설치되는 파일** 이름을 기반으로 탐지
- /sbin/su, /system/su, /system/sbin/su, /system/xbin/su, /system/app/Superuser.apk

### 시스템 설정 파일 확인

- 루팅 후 변경이 가능한 시스템 **설정 파일** 확인
- ro.debuggable, service.adb.root

<br><br>

## 프로그램 무결성 검증

변조된 프로그램이 정상 실행될 경우, 악성코드가 포함된 재 배포되는 파일 등의 보안 위협이 존재함에 따라,

**변조 프로그램 정상 실행 가능** 여부 점검

<br><br>

## 메모리 내 중요정보 노출 여부

이용자 단말기 메모리 영역에서 이용자 중요정보의 **평문 노출 여부**를 점검

<br><br>

## 단말기 내 중요정보 저장 여부

애플리케이션 사용 폴더 및 외부 저장소에 존재하는 파일 내 **중요정보 저장 여부** 점검

<br>

### SharedPreferences

- 애플리케이션 폴더 내 데이터를 **파일로** 저장
- /data/data/<패키지이름>/shared_prefs/ → cat 명령어로 파일 열람

### SQLite

- 애플리케이션 폴더 내 데이터를 **DB 파일로** 저장
- /data/data/<패키지이름>/databases/ → DB Browser for SQLite 프로그램으로 확인 가능

<br><br>

## 화면 강제 실행에 의한 인증단계 우회

화면 강제 실행, 인증관련 파일 조작 등을 통해 **인증단계 우회 가능** 여부 점검

<BR>

### AndroidManifest.xml 내 exported 속성 존재 여부 확인

AndroidManifest.xml 내 별도 명시가 없는 경우 **false**로 간주

- android:exported=true → **다른** 앱이나 **시스템**에서 해당 서비스 직접 접근 가능
- android:exported=false → 앱 **내부**에서만 접근 가능
- `adb shell am start -n <android:name 강제 실행하고자하는 페이지>`

<br><br>

# 2. APK 동적 분석

![image-20221125150513562](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221125150513562.png)

앱 **실행중인 환경**에서 결함을 찾아내는 기법이다.

**사용자 입/출력** 및 **실행 흐름 변조** 등을 통해 취약점 발생 가능성 확인하는 기법이다.

IDA를 통해 **DEX**, **SO**파일들을 동적 분석할 수 있다.

동적 분석을 하기 위해서는 2가지 **조건** 중 하나가 만족해야 한다.

- AndroidManifest.xml 파일 내 **android.debuggable=true** 설정
- 모바일 기기의 **ro.debuggable=1** 설정

<br>

## DEX

Android 런타임에서 궁극적으로 **실행하는 코드**가 포함되어있는 파일

기계어로 설정되어있으며, 디컴파일 시 **smali** 코드로 변환된다.

<br>
<br>

## SO

Android 에서 Native Library 단에서 사용하는 C/C++로 작성된 **시스템과의 연계**를 위해 사용하는 파일

/lib/arm64-v8a. armeabi-v7a, x86에 위치

<br>

### loadLibrary

​		Static{

​				System.loadLibrary("so file name");

​		}

<br><br>

## MultiDex

Dex 사이즈 제한으로 큰 앱 같은 경우 **여러개**의 Dex를 사용하는데,

이러한 Dex들을 **동적으로 로드**하기 위해 MultiDex를 사용한다.

Dex 파일을 획득한다면, 원본 소스코드에 가깝게 **복호화**가 가능하다.

따라서, APP 소스코드가 담긴 Dex파일을 **숨기는 것**은 매우 중요하다고 할 수 있다.

<br>

### 취약점

악성 소스코드 및 솔루션 소스코드를 원본 Dex를 숨겨놓은 위치에서 **정상적인 Dex인 것처럼** 보여주고,

실제 원본 Dex같은 경우는 따로 **숨겨놓는** 취약점이 있다.

<br>

### Dex파일 동적 호출

File에서 호출

- `Dalvik.system.DexFile.loadDex deprecated after API 26`
- `Dalvik.system.DexClassLoader`
- `Dalvik.system.PathClassLoader`

Memory에서 호출

- `Dalvik.system.InMemoryDexClassLoader`

<br>

### ro.debuggable

루팅이 되면 **System Property** 변경을 값을 통해서 디버그 기본 설정 변경(루팅 탐지)한다.

- ro.debuggable=1 : 앱 실행 시, **debug** 모드로 실행
- ro.debuggable=0 : 앱 실행 시, 앱에 명시된 **debuggable** 속성으로 실행(default는 false)
- ro.secure=1 : adb **유저** 권한 실행
- ro.secure=0 : adb **루트** 권한 실행