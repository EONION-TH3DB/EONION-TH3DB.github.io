---
title: "[bWAPP] 7. Missing Functional Level Access Control - Restrict Device Access"
---

# Definition

접근 통제와 확인이 서버의 설정이나 관리 측면에서 이루어지지 않을때 발생하는 취약점

대표적으로 파일 다운로드와 업로드 취약점을 이용하여 웹 서버에 접근하는 공격

# Restrict Device Access

웹 사용자 브라우저에서 모바일 페이지로의 접근을 제한

보안 측면 상 PC 단말에서 데이터를 조작하는 행위를 방어하기 위한 설정



## Object

모바일에서만 동작하는 페이지를 PC로 접근하여 공격을 시도할 수 있다는 것을 알아보자



## 시나리오

![image-20211103205657707](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103205657707.png)

- PC로 접근하기 때문에 모바일이 아니라는 경고 메시지
- Chrome에서 User Agent Switcher라는 플러그인 다운로드



## User Agent Switcher

![image-20211103211059755](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103211059755.png)

![image-20211103211213486](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103211213486.png)

- Chrome Web Store에서 User Agent Switcher 플러그인 설치
- 운영체제 IOS 선택
- iPhone 6로 모바일 설정

![image-20211103211323001](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211103211323001.png)

- User Agent 헤더 정보 변경이 완료되어 모바일 버전으로 접속
- PC 버전에서 보이지 않던 메시지 확인

# 대응방안

공격자는 모바일에서만 동작하는 페이지를 통해 역으로 공격 시도 가능

User Agent 조작이 매우 쉽기때문에 모바일용 페이지를 별도로 개발하고 기능을 간단하게 하여 웹 페이지에서 발생하는 동일 취약점을 차단하는 방법 권고
