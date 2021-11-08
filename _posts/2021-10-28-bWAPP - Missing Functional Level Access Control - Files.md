---
title: "[bWAPP] 7. Missing Functional Level Access Control - Directory Traversal - Files"
---





## Missing Functional Level Access Control

접근 통제와 확인이 서버의 설정이나 관리 측면에서 이루어지지 않을때 발생하는 취약점

대표적으로 파일 다운로드와 업로드 취약점을 이용하여 웹 서버에 접근하는 공격









## Directory Traversal

주로 파일 다운로드 취약점과 같은 뜻으로 사용

상대경로나 기본으로 설정된 파일명, 디렉터리명을 통해 접근을 허용하지 않은 디렉터리나 파일에 접근

때문에 공격자는 시스템과 DB의 정보를 수집 가능









## Files

### Object

GET 메소드를 사용하는 웹 페이지(bwapp - Files)의 특성을 이용해 서버 디렉토리에 있는 파일의 정보를 알아보자





### 시나리오

![image-20211028211704174](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028211704174.png)

- 이전 게시물인 Directories에서 확인했다 싶이 이번 시나리오 또한 HTTP 연결 요청으로 GET 방식 사용하여 URL에 변수 노출
- 상대경로를 사용해보자





### ../../../etc

![image-20211028211940326](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028211940326.png)

- 파일을 찾는 page라는 변수의 기능 때문에 이전 게시물 Directories와는 달리 변수 값에 디렉토리를 조회할 시 아무것도 뜨지 않음
- Directories에서 열어보고자 했던 passwd 파일을 변수 값에 ../../../etc/passwd 라고 입력하고 열어보자





### ../../../etc/passwd

![image-20211028212302408](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028212302408.png)

- 서버에 접근할 수 있는 ID와 접근 권한 등을 기록한 파일이라는 것 확인
- 저장된 파일의 디렉터리와 파일명을 알고 있다면 원하는 DB 정보 알아낼 수 있음









## 대응방안 - Beebox

### Beebox - 1

![image-20211028212809641](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028212809641.png)

- 디렉토리 변경
- vi 편집기로 해당 웹 페이지 소스코드 확인





### Beebox  - 2

![image-20211028213023725](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028213023725.png)

- Security Level 확인





### Beebox - 3

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028213310524.png" alt="image-20211028213310524" style="zoom:80%;" />

- "case 0 : 난이도 하"에서 아무런 보안 조치 X
- "case 0 : 난이도 중"에서 directory_traversal_check_1 함수 사용해 변수에 입력한 데이터가 올바른 데이터인지 확인
- "case 0 : 난이도 상"에서 directory_traversal_check_3 함수 사용해 변수에 입력한 데이터가 올바른 데이터인지 확인





### Beebox - 4

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028213658362.png" alt="image-20211028213658362" style="zoom:102%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211028213804708.png" alt="image-20211028213804708" style="zoom:70%;" />

- directory_traversal_check_1 에서 상대경로에 사용하는 특수문자를 검사 후 오류 메시지 출력
- directory_traversal_check_3 에서 realpath 함수 호출하여 상대경로를 절대경로로 반환 후, strpos 함수로 사용자가 입력한 경로가 기본 경로에 포함되는지 확인 후 오류 메시지 출력