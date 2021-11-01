---
title: "[bWAPP] 7. Missing Functional Level Access Control - Remote & Local File Inclusion (RFI/LFI)"
---

# Definition

접근 통제와 확인이 서버의 설정이나 관리 측면에서 이루어지지 않을때 발생하는 취약점

대표적으로 파일 다운로드와 업로드 취약점을 이용하여 웹 서버에 접근하는 공격

# File Inclusion

악의적인 코드가 입력된 파일을 공격자가 서버에 업로드하여 사용자가 열람하는 공격

# RFI(Remote File Inclusion)

공격자가 악성 코드가 있는 원격 서버의 파일을 공격 대상인 웹 애플리케이션 서버에서 실행시켜 취약한 웹 페이지에 악의적인 스크립트를 실행하게 하는 공격

# LFI(Local File Inclusion)

서버 내부에 있는 파일을 확인하는 공격

서버에 접근하는 변수 중 취약한 변수에 상대경로(../)를 사용하여 서버 내부에 접근

# Local File Inclusion (RFI/LFI)

## Object

GET 방식으로 HTTP 요청을 사용하는 웹 페이지의 URL에 상대경로(../)를 이용하여 정보를 갈취해보자



### 난이도 : 하

![image-20211101194815125](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211101194815125.png)























