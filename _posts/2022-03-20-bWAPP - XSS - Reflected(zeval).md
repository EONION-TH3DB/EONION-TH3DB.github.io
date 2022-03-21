---
title: "[bWAPP] 3. XSS - Reflected (Eval)"
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

## Eval

'eval' 함수는 수식 형태로 된 문자열을 실수로 반환하는 자바스크립트 내장함수이다.

작은 따옴표에 의한 값은 보통 텍스트 문자로 처리되지만, eval 함수를 사용하면 PHP 코드로 해석하고 실행한다.

<br>

<br>

## XSS - Reflected (Eval) - 난이도 : 하

![image-20220321123224763](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321123224763.png)

![image-20220321123300251](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321123300251.png)

- 현제 날짜 정보를 GET 메서드를 통해 URL의 date 변수로 받아온다.
- date 변수에 eval 함수를 넣어보자

```php
eval(document.write(document.cookie))
```

<br>

![image-20220321123620212](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321123620212.png)

- 쿠키 값 출력
- 난이도 : 중 또한 마찬가지로 진행된다.
- 난이도 : 상에서는 진행이되지 않는데 대응방안에서 확인해보자.

<br><br>

## 대응방안

### Linux

![image-20220321172337025](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321172337025.png)

- vi 편집기를 통해 xss_eval.php 소스코드를 열어주자.
- 난이도 상일때만 보안이 적용되어있다.
- date 변수 값으로 Date() 값이 아니면 전부 오류처리하고 있다.
