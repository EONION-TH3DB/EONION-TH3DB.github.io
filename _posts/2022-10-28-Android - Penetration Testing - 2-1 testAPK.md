---
title: "[Android] 루팅 탐지 우회(NOX) - 2. 진단"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

# **2. 진단**

- 애플리케이션 APK 추출
- Decompile
- jd-gui로 탐지 로직 확인
- NotePad++로 smali 코드 변조
- Compile
- 키생성
- 서명
- Install
- 결과
- 에러 문구

<BR>

<BR>

<BR>

## APK 추출

![image-20221028160720768](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221028160720768.png)

애플리케이션 좌측 마우스로 1초정도 눌러준 후 [**추출**] 버튼 클릭

<BR>

![image-20221028160857739](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221028160857739.png)

Nox가 설치된 경로에 <애플리케이션이름>.apk로 **추출**된 것 확인 가능

<br>

![image-20221028161128291](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221028161128291.png)

해당 파일 우클릭해서 [**압출 파일 미리 보기**]에 있는 **classes.dex** 만 압축을 풀어 주자.

자 이제 압축을 푼 **dex**파일을 **class**파일로 **디컴파일** 해주겠다.

<BR>

<BR>

## **Decompile**

### dex2jar 다운로드

![image-20221031193158074](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031193158074.png)

[https://github.com/pxb1988/dex2jar](https://github.com/pxb1988/dex2jar)

해당 사이트에서 **dex2jar**를 다운로드 받아주자

<br>

![image-20221031193358022](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031193358022.png)

APK 파일에서 추출한 dex 파일을 dex2jar 폴더 **디렉토리**에 넣어주자

<br>

![image-20221031193704815](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031193704815.png)

CMD 창을 열어서 dex2jar 폴더 **디렉토리**로 설정해주고

`d2j-dex2jar.bat <파일이름.dex>`

위와 같이 명령어를 입력해주면 **java 소스파일**을 얻을 수 있다.

<br>

<br>

## **jd-gui로 탐지 로직 확인**

![image-20221031194826285](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031194826285.png)

dex2jar 툴로 디컴파일한 jar 파일을 jd-gui 툴로 열어주면 위와 같이 **java 소스코드**를 확인할 수 있다.

함수 명이 **'a'**인 것과 루팅 탐지 구문이 보인다.

함수 명이 **'a'**인 이유는 **난독화**가 적용되어 있어 함수명이 대체 **치환**되어 있기 때문이다.

자 이제 'a'라는 함수 부분을 **변조**하여 루팀 탐지를 우회해보자.

<br>

### apktool

![image-20221031200759725](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031200759725.png)

1-2강 [https://eonion-th3db.github.io/Android-Setting-1-2-JAVA/](https://eonion-th3db.github.io/Android-Setting-1-2-JAVA/) 에서

다운로드 받았던 apktool 파일을 **Uncrackable1.apk 경로**에 넣어주고

CMD에서 위 경로로 위치 시켜주자

<br>

![image-20221031201118137](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031201118137.png)

![image-20221031201504851](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031201504851.png)

java -jar apktool_버전.jar d <파일이름>.apk``

- `d : 디컴파일 옵션`
- `jar : jar 파일로 압축되어 있는 자바 프로그램을 실행`

명령어를 실행해주면 APK 파일이 **디컴파일**되어 폴더가 생성된다.



<BR>

![image-20221031204308940](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031204308940.png)

해당 경로로 타고 들어오면 

루팅 탐지 문구가 있었던 **MainActivity** 파일을 발견할 수 있다.

자 이제 **MainActivity.smali** 코드를 변조하자

<br>

<br>

## **NotePad++로 smail 코드 변조**

![image-20221031210223570](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031210223570.png)

**MainActivity.smali** 파일을 NotePad++로 열어보면 루팅 탐지 구문 확인 가능하다.

<br>

![image-20221031210429260](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031210429260.png)

**Root detected** 라는 구문이 안나오게하면 루팅 우회 가능할 걸로 판단

다음과 같이 바꿔주자

<br>

![image-20221031210626067](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031210626067.png)

**분기**로 판단되는 두 곳

if_nez v0, :cond**_0** 를 if_nez v0, :cond**_1**로 바꿔주자

<br>

<br>

## **Compile**

![image-20221031210920921](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031210920921.png)

`java -jar apktool_<버전>.jar b <APK파일 디컴파일한 폴더>`

- `b : 컴파일`

수정한 smali 파일이 있는 폴더를 다시 APK 파일로 **컴파일** 해주자.

<BR>

![image-20221031211037313](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221031211037313.png)

코드 변조 했던 **Uncrackable1** 폴더 안에 **dist**라는 폴더에 컴파일한 **APK** 파일 생성된 것 확인

<br>
<br>

## 키생성

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTML4cb4e2d8.PNG)

`keytool -genkey -v -keystore <파일명>.keystore -alias <별칭> -keyalg RSA -keysize 2048 -validity 10000`

- `genkey : 키(비밀키, 공개키) 생성`
- `verbose : 상세 정보 화면에 표시`
- `keystore : 비밀키와 증명서를 저장하는 키스통의 파일경로 및 파일명 입력`
- `keyalg : 키를 생성하는 알고리즘 지정 (DSA, RSA)`
- `keysize : 키 값 길이`
- `validity : 인증서의 유효기간`

<br>

<br>

## **서명**

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTML4cb445fc.PNG)

`jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore <키 파일명>.keystore <apk파일명>.apk 별칭`

- `sigalg : 시그니쳐 알고리즘`
- `digestalg : 128비트 고정 길이로 메시지를 압축하는 단방향 해시 알고리즘`
- `verbose : 상세 정보 화면에 표시`

<BR>

sigalg 과 digestalg 을 **SHA1**으로 설정 시 (**disabled**) 표시로 서명이 되지 않는 걸 확인할 수 있다.

**jdk.jar.disableAlgorithms** 으로는 **MD2**, **MD5**, 1024 미만 키 길이의 **RSA**, 1024 미만 키 길이의 **DSA**, **SHA1** 이 있으며,

보안 상 문제로 **2019-01-01** 이후 **거부**되어 있다.

<br>

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTML4cb7b8ad.PNG)

![image-20221101210353659](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221101210353659.png)

`jarsigner -verify -verbose -certs <apk파일명>.apk`

- `verify : 서명된 jar 파일 확인`
- `certs : 진행 및 인증 과정에서 인증서 화면에 표시`

위 명령어를 통해서 **서명** 됐는지 **확인**.

<br>

<br>

## **Install**

![image-20221101212238690](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221101212238690.png)

![image-20221101212319533](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221101212319533.png)

먼저, 녹스 내에 설치되어있는 기존 앱 **삭제**.

`nox_adb install <서명된apk파일명>.apk`

위 명령어로 **설치**하거나 **드로그 앤 드랍**으로 설치해주자.

<br>

<br>

## 결과

![img](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/SNAGHTML4cccb8f5.PNG)

루팅 탐지 우회 **성공!**

**3강**에서는 앱 메모리 내에 노출되는 **중요 정보**를 찾아보겠다.

<br>

<br>

## 에러 문구

**install / uninstall** 시 발생되는 에러 문구에 대해 알아보자.

1. **Success** :
   - 정상 설치
2. **error : more than one device and emulater**
   - apk install 시 너무 **많은 단말기 **연결되어 있는 경우이다.
   - **하나**의 단말기만 연결해주면 해결 가능하다.
3. **Failure [INSTALL_FAILED_ALREADY_EXISTS]**
   - 단말기에서 해당 패키지 앱을 **삭제**하고 다시 설치하자.
4. **Failure [INSTALL_PARSE_FAILED_NO_CERTIFICATES]**
   - **재설치** 시 (디컴파일 후 컴파일) 나타날 수 있는 에러다.
   - **서명**까지 완료했지만 설치가 되지 않는 경우 이런 오류가 뜨는데
   - 이는 **사인 키 값이 서로 맞지 않아 발생**한다.
   - jarsigner로 서명 과정에서 sigalg 값과 digestalg 값을 **동일**하게 해주고, 명칭도 **정확**하게 입력해주자
     - ex) -digestalg SHA-256 (O) / -digestalg SHA256(X)
