---
title: "[bWAPP] 3. XSS - Reflected (PHP_SELF)"
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

## PHP_SELF

웹 서버가 실행 중인 현재 웹 페이지의 경로를 나타내는 $_SERVER의 속성이다.

$_SERVER['PHP_SELF'] 형태로 사용하며,

이 때 $_SERVER는 웹 서버의 헤더와 경로 등의 정보를 포함하는 배열이다.

$_SERVER['PHP_SELF'] 는 URL을 웹 페이지의 경로로 인식하고 받아들이기 때문에 URL에 악성 스크립트를 입력할 경우 XSS에 매우 취약하다.

<br>

<br>

## XSS - Reflected (PHP_SELF) - 난이도 : 하

![image-20220321133235802](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321133235802.png)

- 이름 입력하는 시나리오

<br>

![image-20220321135720484](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321135720484.png)

- 아무거나 입력하면 URL에 GET 메서드로 전송하여 변수가 나타나는 것을 확인할 수 있다.

<BR>

### xss_php_self.php

![image-20220321135411909](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321135411909.png)

- form 태그를 확인해보면 action 속성에 $_SERVER["PHP_SELF"]를 사용하여 현재 페이지의 경로를 반환하고 있다.
- 또한, GET 메서드를 활용하여 전송한다는 것도 확인했다.
- 그러면 URL에 다음과 같은 코드를 삽입하자.
- form 태그를 가엦로 닫기위해 먼저 "/>를 입력해주고 스크립트를 삽입하자.

```apl
http://172.30.1.16/bWAPP/xss_php_self.php/"/><script>alert('WARNING')</script>
```

<br>

![image-20220321140104437](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321140104437.png)

<br>

<br>

## XSS - Reflected (PHP_SELF) - 난이도 : 중

```apl
http://172.30.1.16/bWAPP/xss_php_self.php/"/><script>alert(document.cookie)</script>
```

- 난이도 하에서 진행했던 코드를 삽입했더니 진행되질 않아서 난이도 중에서는 작은 따옴표와 큰 따옴표를 필터링하고 있다고 추측할 수 있다.
- 그래서 다른 코드를 삽입해봤다.

<br>

![image-20220321140428146](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321140428146.png)

- 성공

<br>

<br>

## 대응방안

### Linux

![image-20220321163004306](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321163004306.png)

- vi 편집기로 xss_get.php 소스코드를 열어보자

<br>

### 난이도 : 중

![image-20220321161425578](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161425578.png)

- vi 편집기로 functions_external.php 소스코드를 열어보자.
- 난이도 중 은 데이터를 xss_check_4로 부터 받아오는데 최종적으로 addslashes를 반환하고 있다.
- adslashes
  - 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수

<br>

### 난이도 : 상

![image-20220321161652371](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161652371.png)

- 난이도 상 에서는 데이터를 xss_check_3 로부터 받아오는데 최종적으로 htmlspecialchars를 반환한다.
- htmlspecialchars
  - 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함.
