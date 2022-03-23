---
title: "[bWAPP] 3. XSS - Reflected (HREF)"
---

<br>

## Cross Site Scriptin (XSS)

웹 사이트 관리자가 아닌 이가 웹 페이지에 악성 크릡트를 삽입하는 취약점.

주로 전자 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어짐.

웹 애플리케이션이 사용자로부터 입력받은 값을 제대로 검사하지 않고 사용할 경우 나타나는데

주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 한다.

<br>

<br>

## Reflected XSS

웹 페이지에 존재하는 파라미터에 악의적인 스크립트 코드를 입력하여 사용자가 URL을 클릭하면 파라미터에 입력한 악성 스크립트 코드가 실행되게 하는 공격.

스크립트가 포함된 URL을 메일로 전송하거나 게시물로 등록하여 사용자가 클릭하도록 유도한다.

사용자들이 알아채기 쉽고 일부 브라우저에서 악의적인 스크립트에 대응하고 있기 떄문에 Stored XSS 보다 위협이 적지만

악성 서버로 유도되면 Stored XSS와 위협은 동일하므로 입력 값 검증이 필요하다.

<br>

<br>

## a 태그

문서에서 다른 문서로 이동하는 하이퍼텍스트 기능을 제공.

a 태그를 사용할 수 있는 속성은 href, title, target 등이 있으며 onClick, onmouseover 등의 이벤트 속성을 사용하여 하이퍼링크로 다른 페이지로 이동할 때 다양한 방법을 제공한다.

<br>

<br>

## XSS - Reflected (HREF) - 난이도 : 하

### xss_href-1.php

![image-20220321124154845](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321124154845.png)

<br>

### xss_href-2.php

![image-20220321124315478](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321124315478.png)

- 아무 이름이나 입력하고 들어오게 되면..

<br>

### xss_href-3.php

![image-20220321124401954](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321124401954.png)

- vote를 클릭하게되면 해당 페이지로 들어오게 된다.
- 그렇다면 a태그를 활용해서 xss_href-2 페이지의 vote 버튼에 마우스를 올리게되면 메시지를 나타내고 타 사이트로 이동시켜보자.

<br>

### xss_href-2.php 소스코드

![image-20220321124616838](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321124616838.png)

- vote를 클릭시 xss_href-3.php 이동하게되는 하이퍼링크 태그 a가 걸려있다.
- 변수 movie, name, action 중에 xss_href-1.php에 입력하는 폼으로 받는 name 변수에 다음과 같이 삽입하자.

```html
0 onmouseover=alert(document.location='http://172.30.1.16/bWAPP/xss_href-1.php') a
```

- onmouseover 이벤트 속성으로 vote 위에 마우스 커서를 얹었을때 'http://172.30.1.16/bWAPP/xss_href-1.php' 메시지와 함께 자동으로 해당 사이트로 이동되게 document.location 함수를 이용했다.
- 이동하게되는 페이지는 아주 위험한 사이트라고 가정해주시길 바란다.
- 또한, 0,a 는 변수의 입력값으로 사용하여 오류를 막아주자.

<br>

![image-20220321125431147](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321125431147.png)

- 먼저 이렇게 alert가 뜨고 확인을 누르게되면

<br>

![image-20220321125501848](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321125501848.png)

- 설정해 놓은 페이지로 이동하게 되는 취약점을 발견할 수 있다.

<br>

<br>

## 대응방안

### Linux

![image-20220321172806730](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321172806730.png)

- 난이도 중과 상에서는 name 변수에 a 태그의 속성을 삽입해도 태그가 실행되지 않는다.
- vi 편집기로 xss_href-2.php 소스코드를 확인해보면 xss_href-3,php를 호출하는 하이퍼링크에서 name 변수를 인자로 입력하여 hpp 함수 호출한다.
- 난이도 하를 제외하고는 모두 urlencode 함수를 사용하여 취약점에 대응하고 있다.
- Urlencode
  - 0-9, a-z, A-Z, -, _ 를 제외한 특수문자를 URL 인코딩하는 함수
  - 따라서 스크립트 코드에 사용하는 (, ), ', 등의 문자가 인코딩되어 코드가 실행되지 않는다.
  - 이는 민감한 정보를 다루지 않는 페이지에서 하이퍼 링크를 보호하는 방법이다.

<BR>

<BR>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습
