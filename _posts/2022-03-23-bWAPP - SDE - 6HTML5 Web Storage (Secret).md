---
title: "[bWAPP] 6. Sensitive Data Exposure - HTML5 Web Storage (Secret)"
---

<br>

## Sensitive Data Exposure

- 클라이언트와 서버가 통신 시 SSL(암호화 프로토콜)을 사용하여 중요한 정보를 보호하지 않을 때 발생하는 취약점.
- 사용자가 민감한 정보를 입력할 때 암호화 저장이 이루어지지 않으면 공격자가 중간에서 정보를 탈취할 수 있다.
- 또한, 데이터 처리와 암호화 저장이 클라이언트 측에서 이루어질 경우 공격자는 클라이언트 PC를 장악하여 정보 탈취가 가능하므로 데아터 처리와 암호화는 반드시 서버 측에서 이루어지게끔 해야 한다.

<br><br>

## HTML5 Web Storage (Secret)

HTML5는 웹 저장소를 사용하여 쿠키를 대체한다.

기존 쿠키와는 다르게 서버와의 통신 과정에서 매번 쿠키를 전송하지 않기 때문에 네트워크에서 안전하다는 장점이 있지만, 클라이언트에서 정보를 수정할 수 있어서 XSS 공격이 가능하다.

따라서 XSS 취약점이 있는 xss_get.php 페이지를 활용해 HTML5 웹 저장소에 있는 내용을 노출시켜 보자.

<BR>

![image-20220323205947607](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323205947607.png)

- 로그인 이름과 시크릿이 HTML5 웹 스토리지에 저장되어있단다.
- XSS를 사용해 도전해보라고 한다.

<BR>

### xss_get.php

![image-20220323210122404](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323210122404.png)

- 이쪽 시나리오에다가 HTML5에 저장되어 있는 내용을 출력해보자
- 다음과 같은 코드를 삽입하자

```html
<script>
for (var key in localStorage) {
  document.write(key + " = " + localStorage[key] + "<br>");
}
</script>
```

- key 값들을 웹페이지에 출력하는 DOM 기반의 함수 documen.wrtie를 사용해 First name에 입력해주고 Last name에는 아무거나 입력해보자.

<br>

![image-20220323210424263](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323210424263.png)

- 다음과 같이 Key로 설정되어있는 여러 로그인 값들과 개인정보 값들이 유출된 것을 확인할 수 있다.

<br>
<br>

## 대응방안

![image-20220323215458695](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220323215458695.png)

- 난이도 상에서는 로컬 저장소에 저장된 비밀번호 힌트를 출력할 때 SHA-1 해시 함수의 반환 값으로 사용한다.
- 그러나 난이도 상에는 로컬 저장소 사용 여부를 알 수 있는 변수나 XSS 취약점을 제공하는 페이지가 없기 때문에 취약점 여부를 판단하기 어렵다.
- 또한, 아예 로컬 저장소를 사용하지 않는 경우 웹 저장소에서 출력할 정보가 없기 때문에 취약점이 존재하지 않는다고 볼 수 있다.
