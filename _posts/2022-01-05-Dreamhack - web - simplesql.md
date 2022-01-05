---
title: "[Dreamhack] War Game 1단계 - Web Simple_SQLI"

---

<br>

## 문제

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191040382.png" alt="image-20220105191040382" style="zoom:80%;" />

<br><br>

## 웹 페이지

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105195103715.png" alt="image-20220105195103715" style="zoom:120%;" />

- SQL Injection 중 sqli에 관한 문제

<br>

![image-20220105191231785](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191231785.png)

- login 폼에서 Injection하는 문제인 것 같다.

<br><br>

## 로그인

![image-20220105191420405](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191420405.png)

![image-20220105191440812](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191440812.png)

- 로그인 실패 시 나오는 값 확인

<br>

![image-20220105191544953](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191544953.png)

![image-20220105191559089](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105191559089.png)

- 로그인 성공 시 나오는 구문 확인
- userid 폼에 무조건 참이 되는 식을 넣어 참 값 도출
  - "select all from table where column = (userid에 사용자가 입력하는 값)" or 1=1 --
  - 대략 이런 식으로 구성되어있는 걸 " 큰 따옴표로 닫고 or 로 연결하여
  - 1=1 을 식에 붙여 무조건 참이되는 식을 작성한다.
  - sqlite의 주석처리는 # 이 아닌 -- 이다.

<br><br>

## 컬럼 갯수 확인

![image-20220105192134425](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105192134425.png)

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105192151230.png" alt="image-20220105192151230" style="zoom:102%;" />

- union select 이용해서 컬럼 갯수가 몇개인지 숫자를 대입하여 알아보자
  - 숫자를 대입하는 이유는 결과 메시지나 칸에 몇 번 숫자가 도출되는지 확인하여 Injection한 구문에서의 숫자를 통해 컬럼의 위치를 확인하기 위함이다.
- " union select all 1 from sqlite_master --
  - 1개는 아님
  - sqlite_master 는 MySQL 기준 Information_schema(DB의 메타정보) 임

<br>

![image-20220105192902397](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105192902397.png)

- " union select all 1, 2 from sqlite_master --
  - 2개의 컬럼으로 확인되며 컬럼을 도출하는 위치는 첫 번 째임 1 임

<br><br>

## 테이블 이름

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105194408737.png" alt="image-20220105194408737" style="zoom:94%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105193313935.png" alt="image-20220105193313935"  />

- 결과를 도출하는 위치인 1 번에 tbl_name을 입력해서 테이블 네임 알아보자
  - tbl_name 은 MySQL 기준 table_name 임
- 테이블 이름은 users

<br><br>

## 테이블 필드 정보(컬럼 정보)

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105193457231.png" alt="image-20220105193457231" style="zoom:95%;" />

- 1번에 sql 을 넣어주고 where 로 테이블 이름을 조건으로 붙여준다.
  - sql 은 해당 테이블의 필드 정보

<br>

![image-20220105193650828](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105193650828.png)

- 컬럼이 userid(100글자 제한), userpassword(100글자 제한)인 users 테이블을 생성한다.

<br><br>

## 필드(컬럼) 내용 확인

![image-20220105194014022](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105194014022.png)

- userid 컬럼의 정보 확인

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220105194045690.png" alt="image-20220105194045690" style="zoom:94%;" />

- DH{1f136225e316add7bff3349ab1dd5400}