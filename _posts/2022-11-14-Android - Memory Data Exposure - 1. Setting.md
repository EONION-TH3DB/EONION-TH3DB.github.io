---
title: "[Android] 메모리 내 중요정보 노출(NOX) - 1. 환경설정"
---

<br>

![image-20221025130114073](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221025130114073.png)

# **1. 환경설정**

- 파이썬 설치 - 3.7.4
- 프리다 서버 설치 - 12.7.4
- 프리다 설치 - 12.7.4
- 프리다 툴즈 설치 - 2.0.0
- 프리덤프3 설치
- astrogrep - 4.3.1 설치

<br>
<br>

<br>

## **파이썬 설치**

![image-20221118131139297](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118131139297.png)

[https://www.python.org/downloads](https://www.python.org/downloads)

다음 사이트에서 3.7.4 버전을 다운로드해 주자.

Frida는 Python 기반으로 제작된 도구이기 때문에 파이썬의 **PIP 기능을 이용해 설치**해야 한다.

<br>

![image-20221118131439196](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118131439196.png)

환경변수 추가하는 체크박스를 체크해주자.

안하면 따로 설정해줘야 함.

<br>

<br>

## **프리다 서버 설치**

![image-20221118130916297](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118130916297.png)

Ole가 개발한 DBI(Dynamic Binary Intrumentation) **프레임워크**

다양한 플랫폼(**Windows, Linux, iOS, Android**)에서 프로세스에 대한 인젝션이 가능하기 때문에 큰 **확장성** 가짐

실행중인 APP에 코드 명령어를 삽입해 프로세스를 **추적, 분석, 모니터링, 디버깅**할 수 있는 도구

**파이썬** 기반으로 동작하며, 버전의 영향을 많이 받으므로 **2.X, 3.X 버전**별로 구분하여 관리해야 함

<br>

![image-20221118132819288](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118132819288.png)

녹스의 운영체제 환경을 알아보자

`nox_adb shell`

`getprop ro.product.cpu.abi`

x86인 32비트의 프리다 서버를 다운로드해주면 된다.

<br>

![image-20221118133715442](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118133715442.png)

[https://github.com/frida/frida/releases](https://github.com/frida/frida/releases)

위 사이트에 접속해서 운영체제에 맞는 환경으로 frida-server파일을 다운로드 받아주자

<br>

![image-20221118134132918](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118134132918.png)

다운로드 받은 압축파일을 해제한 후

압축을 해제한 디렉토리에서 다음과 같은 명령어로 nox에 삽입해주자

`nox_adb push frida-server-버전-androud-x86 /data/local/tmp`

삽입한 후 해당 디렉토리에 잘 들어갔는지 확인해주자

`nox_adb shell`

`cd /data/local/tmp`

`ls`

<br><br>

## **프리다, 프리다툴즈 설치**

![image-20221118135243480](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118135243480.png)

파이썬 설치 과정에서 환경변수 등록에 체크를 했기 때문에

명령프롬프트에서 pip 명령어로 프리다를 설치해주자.

`pip install frida==12.7.4`

<br>

![image-20221118135454400](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118135454400.png)

다음으로는 프리다툴즈를 설치해주자

`pip install frida-tools==2.0.0`

<br>
<br>

## **프리덤프3 설치**

### fridump란?

frida framwork를 사용하는 오픈 소스 메모리 덤핑 도구

Windows, Linux, MacOS X 에서 Android, iOS, Windows 응용 프로그램의 접근 가능한 메모리를 덤프할 수 있는 기능을 가지고 있다.

<br>

![image-20221118135837587](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118135837587.png)

[https://github.com/rootbsd/fridump3](https://github.com/rootbsd/fridump3)

다음 사이트에서 fridump3 압출 파일을 다운로드 받아주자.

<br>
<br>

## **astrogrep 설치**

### **astrogrep 란?**

파일 텍스트 **검색** 툴로써

최근 파일 내 특정 **텍스트**를 찾기 위한 툴이다.

우리는 프리덤프로 추출한 앱 내 메모리 덤프파일들을 AstroGrep을 이용하여 **중요정보**를 찾아볼 것이다.

<br>

![image-20221118143900735](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img2/image-20221118143900735.png)

[https://sourceforge.net/projects/astrogrep/](https://astrogrep.sourceforge.net/)

해당 사이트로 이동해서 4.3.1 버전을 다운로드 받아주자.

<br>

자 이제 환경 설정은 끝났으니

진단하러 가보자.
