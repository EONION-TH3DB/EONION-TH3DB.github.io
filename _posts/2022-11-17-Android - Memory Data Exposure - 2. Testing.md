---
title: "[Android] 메모리 내 중요정보 노출(NOX) - 2. 진단"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

# **2. 진단**

- adb shell로 nox에 삽입했던 frida-server ON
- 녹스에서 실행중인(추출할) 앱 PID 확인
- 추출할 앱에서 개인정보 및 중요정보 입력
- fridump로 앱 내 입력한 중요정보 덤프 뜨기
- AstroGrep로 dump 폴더에서 텍스트 찾기

<br><br><br>

## **frida-server ON**

![image-20221118223052351](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118223052351.png)

`nox_adb shell // 녹스 shell 접근` 

`cd /data/local/tmp // frida-server 다운받은 경로로 이동`

`ls -al // 현 디렉토리의 모든 파일들 권한 확인`

`chmod 777 frida-server-12.7.4-android-x86 // frida-server... 파일에 모든 권한 부여`

`./frida-server-12.7.4-android-x86 & // 프리다 서버 백그라운드 실행`

<br>

### 오류 시 해결방법

![image-20221118224029891](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118224029891.png)

 Unable to start server: Error binding to address: Address already in use

다음과 같이 **이미 사용중인 포트**로 프리다 서버가 실행되지 않는 경우가 생기는데

할당된 프리다 서버 포트가 **활성화**되어 있어서 백그라운드 실행이 꼬이는 것 같다.

이때는 다음과 같이 해주자.

`ps|grep frida-server // 프리다 서버의 포트번호를 확인해주고`

`kill -9 <프리다 서버의 포트번호> // 현재 할당된 프리다서버의 포트번호를 죽여주자`

`./frida-server-server-12.7.4-android-x86 & // 다시 프리다 서버를 백그라운드로 실행해주면 해결`

<br><br>

## 녹스에서 실행중인 앱 PID 확인

![image-20221123144259864](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123144259864.png)

`frida-ps -Ua`

다음과 같은 명령어로 앱 내 데이터 정보를 추출할 실행중인 앱의 **PID 정보를 확인**하자.

-Ua : 현재실행중인 앱 확인

-Uai : 모든 앱 확인

<BR><BR>

## 추출할 앱에서 데이터(개인정보 등) 입력

![image-20221123144621927](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123144621927.png)

추출할 앱에서 **로그인**

<br><br>

## Fridump3 실행

![image-20221123144924039](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123144924039.png)

다운받은 fridump3를 압축해제 한 **디렉토리**에서 명령프롬프트를 실행시켜주자

또는 명령프롬프트에서 해당 **디렉토리**로 설정해주자

<br>

![image-20221123145142296](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123145142296.png)

`python fridump3.py -u -s <앱PID>`

다음 명령어로 덤프를 떠주면 해당 경로에 **dump** 폴더가 생길것이다.

<br><br>

## AstroGrep

![image-20221123145459766](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123145459766.png)

AstroGrep에서 폴더 **아이콘**을 눌러 fridump로 메모리를 추출한 **dump** 폴더를 [폴더 선택] 해준다.

<br>

![image-20221123145938887](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123145938887.png)

Search Text 란에 클라이언트 값이 들어있을만한 **메소드 명**을 입력해준 후 Search를 눌러주자.

<br>

![image-20221123150434597](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221123150434597.png)

첫번째 파일을 클릭하여 입력한 **메소드** 값을 찾아주면,

다음과 같이 입력한 순서대로 메모리가 추출되어 마지막에 사용자의 **비밀번호가 평문으로 노출**되는 것을 확인할 수 있다.

<br>

<br>

이상으로 **앱 내 메모리 중요 정보**를 **메모리 덤프**를 통해 알아보았다.
