---
title: "[CodeEngn] Basic RCE L01"
---

<br>

## L01

HDD를 CD-Rom으로 인식시키는 GetDriveTypeA의 리턴값을 구하는 문제

> GetDriveTypeA Function

- disk drive의 Type을 결정하는 함수

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c2839d6-aec1-409d-9b8b-12cc3e335463/Untitled.png)

> 파일 실행

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e9837a4a-feee-41a4-8f4e-852365f2feae/Untitled.png)

> 확인 - Click

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4258d29a-e13b-4c0a-880c-9be5ae7c79c2/Untitled.png)

> 올리디버거로 파일 실행

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44ad386c-b67b-41a4-ae55-49140dc4ff03/Untitled.png)

- 메시지 박스 확인 시 파일 실행 시 나왔던 문자열 확인
- 실패와 성공 문자열 동시에 확인

> 리턴값 알아보기 위해 0040101D에 Break

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a446e66-752c-42ac-9d86-b36952b66ce1/Untitled.png)

- 레지스터 상태 영역에서 리턴값 확인

  레지스터 용도

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1884691e-070d-4e94-94a5-585e4fc26949/Untitled.png)

- EAX 값 3으로 HDD Return 값 3 도출

> 00401026까지 차례대로 실행(Step over - F8)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97e7fadc-4de5-456b-9aaa-175a5ed632b2/Untitled.png)

- EAX 두번 내리고 ESI 세번 올린 후 JMP 조건 명령어 실행

  JMP 조건 명령어

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e9561c67-def8-4e10-a753-b036ccfe7050/Untitled.png)

- 조건 플래그 값이 1(값이 같음)이면 성공 메시지 0(값이 다름)이면 오류 메시지 확인

> 첫 번째 방법

- 00401026 줄의 어셈블리어 명령 코드에서 JE를 JMP로 수정

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5798da1-c619-4379-b19b-93cd5fcdd3c4/Untitled.png)

> 두 번째 방법

- 00401024 00401026 줄의 어셈블리어 명령 코드에서 비교값 통일

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8077a4bc-3da2-4094-8ea1-470eef9d6de0/Untitled.png)

> 결과