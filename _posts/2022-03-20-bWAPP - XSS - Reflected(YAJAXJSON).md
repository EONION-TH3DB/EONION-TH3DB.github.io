---
title: "[bWAPP] 3. XSS - Reflected (AJAX/JSON)"
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

## XSS - Reflected (AJAX/JSON) - 난이도 : 하

![image-20220321064200704](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321064200704.png)

- AJAX 기능을 활용하여 검색어를 입력하면 해당하는 영화가 DB에 있는지를 확인하고 결과를 바로 출력하는 시나리오이다.

<BR>

### BurpSuite

![image-20220321064433375](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321064433375.png)

- 패킷을 가로체보면 GET 메서드를 통해 xss_ajax_2-2.php 페이지로 넘겨주는 것을 확인할 수 있다.
- 그렇다면 URL에 TITLE 변수를 노출하는 특성을 활용해 다음과 같이 스크립트를 삽입하자.

```html
http://172.30.1.9/bWAPP/xss_ajax_2-2.php?title=<script>alert('Succeed')</script>
```

<br>

![image-20220321064722708](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321064722708.png)

<br>

<br>

## XSS - Reflected (AJAX/JSON) - 난이도 : 중

![image-20220321065000996](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321065000996.png)

- 난이도 하 와 같은 방법으로 시도했지만 다음처럼 바로 출력되는 것을 확인할 수 있다.
- 왜 안되는지는 나도 잘 모르겠다.
- 이미지를 삽입하는 방법을 통해 검색창에다가 삽입해주자.(물론 난이도 하에서도 통한다.)

```html
<img src= x onerror=alert('Success')>
```

<br>

![image-20220321065350895](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321065350895.png)

- 성공
- 따로 보안이 적용되어 있지는 않은 것 같다.

<br>

<br>

## 대응방안

### Linux

![image-20220321172106983](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321172106983.png)

- vi 편집기를 통해 xss_ajax_2-2.php를 열어주자.
- 난이도 상일 때만 보안이 적용되어 있다.

<br>

### vi functions_external.php

![image-20220321161652371](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161652371.png)

- 난이도 상 에서는 데이터를 xss_check_3 로부터 받아오는데 최종적으로 htmlspecialchars를 반환한다.
- htmlspecialchars
  - 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함.
