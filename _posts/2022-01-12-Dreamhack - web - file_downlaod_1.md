---
title: "[Dreamhack] War Game 1단계 - Web File-Download-1"

---

<br>

## 문제

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231709810.png" alt="image-20220112231709810" style="zoom:120%;" />

<br><br>

## 접속 정보

### Home

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231952948.png" alt="image-20220112231952948" style="zoom:102%;" />

<br>

### Upload My Memo

![image-20220112225553088](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112225553088.png)

<br>

### test 파일 조회(다운로드)

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112230204152.png" alt="image-20220112230204152" style="zoom:105%;" />

- 클릭시 이런식으로 조회(다운로드)된다.
- url을 확인해보면 GET 방식으로 name 메소드가 쓰인다.

<br>

### flag.py

![image-20220112230459202](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112230459202.png)

- flag.py를 다운받아야하니 flag.py를 만들어 업로드해보자

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112230531242.png" alt="image-20220112230531242" style="zoom:120%;" />

- 뭐 없슴..
- 소스 코드를 좀 살펴보자

<br><br>

## 소스 코드

![image-20220112225457821](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112225457821.png)

- upload_memo()
  - if - with 절에서 filename에 '..'이 들어가면  'bad characters'를 출력하고
  - 업로드 성공시 UPLOAD_DIR/{filename}의 디렉터리로 등록된다.
  - 즉, 업로드된 디렉토리가 UPLOAD_DIR이라는 곳이기 때문에 상위경로(../)를 이용해 이곳에서 flag.py를 다운로드(접속 정보 페이지에서는 조회)

<br>

## 접속 정보

### Upload My Memo

![image-20220112230724268](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112230724268.png)

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231752755.png" alt="image-20220112231752755" style="zoom:105%;" />

- 상대경로(../)를 이용해야 하지만 위 소스코드에서 보았듯이 Filename에 상대경로(.../)가 들어갈시 오류 메시지 출력한다.
- 그렇다면 '소스코드' 보기전 '접속 정보'에서 만들었던 파일로 이동하여 GET 방식의 요청을 이용해 name 매개변수에 상대경로를 이용해보자

### flag.py

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231006994.png" alt="image-20220112231006994" style="zoom:110%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231828942.png" alt="image-20220112231828942" style="zoom:113%;" />

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220112231050270.png" alt="image-20220112231050270" style="zoom:105%;" />

- GET 방식의 웹페이지에서 매개변수인 name을 이용해 상대경로를 이용하니

- 다음과 같이 FLAG 출력

  `FLAG = 'DH{uploading_webshell_in_python_program_is_my_dream}'`