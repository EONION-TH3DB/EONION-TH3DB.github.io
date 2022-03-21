---
title: "[bWAPP] 3. XSS - Stored (Blog)"
---

<br>

## Cross Site Scriptin (XSS)

웹 사이트 관리자가 아닌 이가 웹 페이지에 악성 크릡트를 삽입하는 취약점.

주로 전자 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어짐.

웹 애플리케이션이 사용자로부터 입력받은 값을 제대로 검사하지 않고 사용할 경우 나타나는데

주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 한다.

<br>

<br>

## Stored XSS

Stored XSS는 악의적인 스크립트 코드가 웹에 입력되면서 DB에 저장되는 방식이다.

링크를 이용해 일회성으로 클릭을 유도하지 않고 불특정 다수의 사용자가 공격자의 게시물에 접근하면 지속적으로 악의적인 스크립트가 실행되기 때문에 위협 영향도가 높다.

보통 홈페이지를 해킹해서 내용을 변조하여 스크립트를 삽입하는데 주로 게시판을 이용해 랜섬웨어를 많이 설치한다.

<br>

<br>

## XSS - Stored (Blog) - 난이도 : 하

![image-20220321152850645](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321152850645.png)

- 블로그에 기재할 내용을 적어 게시물을 등록하는 시나리오

<br>

![image-20220321153004632](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321153004632.png)

- 이렇게 출력된다.
- 쿠키값을 출력해주는 스크립트를 삽입해보자.

```html
<script>alert(document.cookie)</script>
```

<br>

![image-20220321153226501](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321153226501.png)

- 스크립트 코드가 저장되어 다른 웹 브라우저로 xss_stroed_1.php 페이지에 접속하면 바로 사용자의 쿠키 값을 알아낼 수 있다.

<br>

## XSS - Stored (Blog) - 난이도 : 중

![image-20220321153416330](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321153416330.png)

- 난이도 : 하 와 같은 방식으로 하면 문제 없음
- 하지만 다음과 같은 코드는 실행이 되지 않는 걸 확인할 수 있는데 난이도 중은 따옴표를 필터링하는 보안함수를 적용하고 있을거라 판단할 수 있다.

```html
<script>alert('WARNING')</script>
```

<br>

<br>

## 대응방안

### Linux

![image-20220321160346217](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321160346217.png)

- vi 편집기를 통해서 xss_stored_1.php 소스코드를 열어주자.
- 난이도 상 중 하 모두 sqli_check_3 함수를 사용하고 있지만 전달받는 데이터가 다르다.

<br>

### vi functions_external.php

![image-20220321161747204](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161747204.png)

- 먼저 공통적으로 쓰이는 sqli_check_3 함수는 mysqli_real_escape_string 함수를 반환하고 있다.
  - mysqli_real_escape_string
    - PHP에서 SQL Injection 공격 등을 방어하기 위하여 특수 문자열을 이스케이프 하기 위한 함수
    - `\x00, \n, \r, \, ', ", \x1a`와 같은 문자 앞에 `\`(역슬레시)를 붙여서 해당 문자가 실제 작동하지 않도록 이스케이프 해준다.
    - addslashes 보다 처리할 수 있는 문자들이 더 많음.

<br>

![image-20220321161425578](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161425578.png)

- 난이도 중 은 데이터를 xss_check_4로 부터 받아오는데 최종적으로 addslashes를 반환하고 있다.
- adslashes
  - 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 난이도 중 에서의 예상이 맞았다.

<br>

![image-20220321161652371](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161652371.png)

- 난이도 상 에서는 데이터를 xss_check_3 로부터 받아오는데 최종적으로 htmlspecialchars를 반환한다.
- htmlspecialchars
  - 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함.
