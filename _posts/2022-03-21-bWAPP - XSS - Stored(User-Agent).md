---
title: "[bWAPP] 3. XSS - Stored (Useer-Agent)"
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

## XSS - Stored (User-Agent) - 난이도 : 하

![image-20220321154633340](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321154633340.png)

- 접속한 웹 브라우저의 정보가 저장된 User-Agent 헤더 값을 최근 접속한 순서대로 3개까지만 테이블 형태로 출력한다.
- 버프수트를 이용해 패킷을 잡고 User-Agent 헤더 부분에 스크립트를 삽입하자.

```html
<script>alert(document.cookie)</script>
```

<br>

### BurpSuite

![image-20220321155447153](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321155447153.png)

- 패킷을 잡아주고 User-Agent에 스크립트를 삽입해준 후 Forward 버튼으로 진행해준다.

<br>

![image-20220321155554698](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321155554698.png)

- 성공
- 다른 사용자로 로그인한 후 해당 페이지에 들어가면 접속한 사용자의 쿠키값이 경고창으로 출력된다.

<br>

<br>

## 대응방안

![image-20220321163004306](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321163004306.png)

- vi 편집기로 xss_stored_4.php 소스코드를 열어보자

<br>

### 난이도 : 중

![image-20220321161425578](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161425578.png)

- vi 편집기로 functions_external.php 소스코드를 열어보자.
- 난이도 중 은 데이터를 xss_check_4로 부터 받아오는데 최종적으로 addslashes를 반환하고 있다.
- adslashes
  - 특수 문자(',",/) 앞에 역슬래시를 삽입하여 특수문자 기능을 문자로 이스케이프하는 함수
  - 그렇기 때문에 난이도 하에서는 다음 구문이 먹혔지만 중 상에서는 먹히지 않았다.

```html
<script>alert('WARNING')</script>
```

<br>

### 난이도 : 상

![image-20220321161652371](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321161652371.png)

- 난이도 상 에서는 데이터를 xss_check_3 로부터 받아오는데 최종적으로 htmlspecialchars를 반환한다.
- htmlspecialchars
  - 메타 문자가 html 태그로 사용되지 않도록 방지하여 입력값 그대로 출력하게 함.
