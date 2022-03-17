---
title: "[bWAPP] 1. Injection - XML/XPath Injection (Search)"
---

<br>

## Injection

공격자가 신뢰할 수 없는 입력을 프로그램에 주입하도록 하는 공격.

<br>

<br>

## XML/XPath Injection

### XML

eXtensible Markup Language의 약어

W3C에서 개발된 여러 특수 목적의 마크업 언어를 만드는 용도에서 권장되는 다목적 마크업 언어

웹 상의 문서를 구조화하고 전송 데이터를 트리 구조의 노드로 표현, 사용자가 정의한 태그로 데이터를 분류 가능

### XPath

XML Path Language

XML 문서의 구조를 통해 경로 위에 지정한 구문을 사용하여 항목을 배치하고 처리하는 방법을 기술하는 언어

<br>

<br>

## Object

XML 구조에 악의적인 구문을 삽입하거나 XPath를 조작하여 XML의 내용을 노출하는 공격 기법

모든 노드를 조회해보자.

XML = DB

XPath = SQL

<BR>

![image-20220317170722629](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317170722629.png)

![image-20220317170747458](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317170747458.png)

- Search 눌렀을 때 GET 방식 URL에 변수 나타남
- 작은 따옴표를 넣어보자

![image-20220317171947610](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317171947610.png)

- XPath 구문 오류 출력
- XPath Injection 가능할 것으로 보임
- 시나리오상 장르를 선택하면 그에 맞는 영화를 나열해주는 것 같다.
- 여기서 XPath 구문을 예상해보자 

```xml
"../hero[contains(genre, '&genre')]/movie"
```

- 실제 현 페이지 소스코드를 열어봐서 확인해보자 -> xmli_2.php

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317172518642.png" alt="image-20220317172518642" style="zoom:150%;" />

- 거의 비슷하다.
  - contains(param, &param) : 두번째인자를 받아와서 첫번째 인자에 있는지 확인하여 참/거짓을 반환한다.

<br>

### 항상 참 구문

```xml
' ) or 1=1 ] [(' a
```

```xml
"../hero[contains(genre, '') or 1=1] [('a')]/movie"
```

- 이런식으로 나오는데
- contains의 괄호와 &genre 변수를 충족시켜준 후 hero 노드의 대괄호 충족시키면 항상 참이된다.
- 또한 뒤 a 부분은 아무의미 없는 그냥 노드인척하여 문제없이 지나가기 위한 것이다.

![image-20220317173112876](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317173112876.png)

<BR>

### 스크립트 삽입

![image-20220317173508096](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317173508096.png)

- 스크립트를 삽입하여 모든 노드를 조회해보자.

```xml
"../hero[contains(genre, '&genre')]/movie"
```

```xml
')] | //* | eonion[('
```

```xml
"../hero[contains(genre, '')] | //* | eonion[('')]/movie"
```

- 장르 변수의 작은따옴표 닫아주기위해 하나 선언해주고 contains의 괄호 닫아주고 hero 노드 대괄호 닫아준다.
  - | : 서로 다른 쿼리들을 연결하는 역할
  - //* : 현재 노드로부터 모든 노드를 조회
  - eonion[('')] : 마찬가지로 의미없는 노드로 만들어주어 정상 쿼리로 작동하게 한다.
- 성공적으로 모든 노드를 조회하여 개인정보를 탈취해보았다.

<br>

<br>

## 대응방안

### Linux

![image-20220318004721601](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318004721601.png)

![image-20220318004752212](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318004752212.png)

- vi 편집기로 xmli_1.php 소스 코드를 열어준다.
- level 1, 2 = Medium, High 에서 xmli_check_1 함수를 사용하고 있다.

<br>

### functions_external.php

![image-20220318004919513](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220318004919513.png)

- 위와 같이 특수문자를 일일이 공백으로 대체해주며 Injection에 대비하고 있다.
