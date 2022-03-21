---
title: "[bWAPP] 3. XSS - Reflected (GET)"
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

## XSS - Reflected (GET) - 난이도 : 하

![image-20220321060627152](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321060627152.png)

- 이름을 입력하는 시나리오에 스크립트를 삽입하여 취약점을 알아보는 시나리오

<BR>

![image-20220321060739015](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321060739015.png)

- 아무거나 입력시 다음과 같이 GET 메서드를 통해 URL에 변수가 노출된다.
- firstname 변수에 다음 스크립트를 삽입해보자.

```javascript
<script>alert('Succeed')</script>
```

<br>

![image-20220321061238828](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321061238828.png)

- 난이도 중과 상에서는 스크립트가 출력되지 않는데 대응방안으로 넘어가서 알아보자.

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
