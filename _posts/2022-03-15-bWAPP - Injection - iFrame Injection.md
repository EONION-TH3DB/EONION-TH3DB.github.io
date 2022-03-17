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

### Linux

![image-20220317223720659](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317223720659.png)

![image-20220317223745531](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317223745531.png)

- vi 편집기로 iframei.php 소스코드 열어준다.
- 레벨확인
- Medium : xss_check_4 함수사용
- High : xss_check_3 함수사용

<br>

### functions_external.php

![image-20220317224046908](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317224046908.png)

![image-20220317221339359](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317221339359.png)

- xss_check_4 에서 addslashes 함수 사용
- addslashes 는 특수 문자(',",/)앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 하지만 인코딩된 태그나 스크립트가 들어오게 되면 바로 디코딩되기 때문에 보안이 뚫린다. 
- xss_check_3 에서는 htmlspecialchars 함수를 사용하고 있는데 
- htmlspecialchars 는 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함으로써 취약점에 대응하고 있다.
