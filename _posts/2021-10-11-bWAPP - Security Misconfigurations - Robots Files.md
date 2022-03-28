---
title: "[bWAPP] 5. Security Misconfigurations - Robots File"
categories: bWAPP
---

<br>

## Security Misconfigurations

잘못된 보안 구성

잘못되거나 과도한 보안 설정으로 발생

<br>

<br>

## Robots File

### Object

- bWAPP의 robots.txt 파일 확인하는 시나리오
- 더 나아가, 구글 해킹의 대응방안에 관한 위협을 알아보자

<br>

### robots.txt

- 페이지에 대한 접근 권한을 설정하는 파일, 웹 크롤러의 접근 범위 설정 가능

|         명령어         |            내용            |
| :--------------------: | :------------------------: |
|     User-agent : *     |       모든 검색 엔진       |
| User-agent : googlebot |       구글 검색 엔진       |
|       Allow : /        |  모든 디렉토리 접근 허용   |
|   Disallow : ex.ini    |  'ex.ini' 파일 검색 차단   |
|   Disallow : /admin/   | 'admin' 디렉토리 접근 차단 |
|       Disallow :       |     사이트 크롤링 허용     |

<br>

### 시나리오 - 난이도 : 하

![image-20211013224743151](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211013224743151.png)

- GoodBot : 사이트 크롤링 허용
- BadBot : 모든 디렉토리 접근 차단
- AllBot : /admin/, /document/, /images/, /passwords/ 디렉토리 접근 불가

<br>

### /admin/ 

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211013234943219.png" alt="image-20211013234943219" title="192.168.15.131/bWAPP/admin/" style="zoom: 60%;" />

- 관리자 페이지에 페이지 인증 과정 없이 직접 접근 가능
- 관리자 계정인 'bee/bug', 내부 IP정보, 서버 정보 노출

<br>

### /psswords/

![image-20211013235122629](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211013235122629.png "192.168.15.131/bWAPP/passwords/")

- wp-config.bak 파일에 접근

<br>

### wp-config.bak

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211014020629609.png" alt="image-20211014020629609" style="zoom:78%;" />

- DB 접속 정보 평문으로 노출

<br>

<br>

## 대응방안

- 중요 디렉터리는 외부에 노출되지 않도록 접근 권한 설정 필요
- 하지만, 페이지에 인증처리를 하지 않을 시 역으로 공격당할 수 있음

<br>

<br>

## 구글 대응

- 모든 디렉터리 검색 제한은 마케팅 차원에서 비효율적
- 앞서 살펴보았듯이 디렉터리 접근 권한 및 애플리케이션 대응방안을 적용하고
- 추가적으로 마케팅을 고려한 대응방안 적용해야 함
