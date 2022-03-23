---
title: "[bWAPP] 3. XSS - Reflected (phpMyAdmin)"
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

## phpMyAdmin

웹 상에서 MySQL을 관리하기 위한 도구로, PHP 언어로 작성되어 있다.

<br>

<br>

## XSS - Reflected (phpMyAdmin) - 난이도 : 하

![image-20220321130113648](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321130113648.png)

- 해당 시나리오는 phpMyAdmin 3.3.8.1/3.3.0. 버전 이하에서 error.php로 XSS 공격이 가능하다.
- CVE-2010-4480 취약점은 @ 문자를 포함하여 조작된 BBCode 태그를 통해 XSS 공격이 가능하다.
- BBCode는 자바 스크립트 및 HTML 인젝션 공격을 방어하기 위해 만들어졌는데, BBCode에서 자바스크립트나 HTML로 변환할 때 XSS 공격이 가능하다.

<br>

![image-20220321130052445](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321130052445.png)

- phpmyadmin에 접속했다.

<br>

![image-20220321130733117](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321130733117.png)

- 또한, 시나리오를 진행할 error.php 로 이동해봤다.
- error.php 에서 사용하는 변수는 type과 error 인데, type은 오류의 종류를 입력해주고,
- error 변수에는 특정 페이지를 호출하는 BBCode 태그를 입력해주는데 @문자를 사용해 [a@URL@Page] 형태로 태그를 조작한다.
  - @page는 지정하지 않거나 문자 값을 입력해준다.
  - 태그 뒤부터 a태그 닫힘 전까지는 BBCode 태그로 @URL 주소로 리다이렉션하기 때문에 하이퍼링크 문자열을 작성해준다.
  - 또한, 설정해준 사이트가 매우 위험한 사이트라고 착각해야한다....

```apl
http://172.30.1.16/phpmyadmin/error.php?type=Bugged!&error=[a@http://172.30.1.16/bWAPP/xss_phpmyadmin.php@]Login[/a]
```

<br>

![image-20220321131932126](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321131932126.png)

- url에 해당 코드를 삽입해주면 다음과 같이 Login 클릭 하이퍼 링크가 나타난다.

<br>

![image-20220321132032254](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321132032254.png)

- Login을 클릭하게 되면 BBCode 태그에 삽입했던 페이지로 이동하게 된다.

<BR>

<BR>

## 대응방안

- 해당 취약점은 phpMyAdmin 3.3.8.1, 3.4.0-beta1 버전에서 발생하므로 버전을 업데이트하여 XSS 취약점에 대비한다.

<BR>

<BR>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습
