---
title: "[CodeEngn] Basic RCE L20"
---

<br>

## L20

### 문제

![image-20220404110652180](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110652180.png)

<br>

### 파일 실행

![image-20220404110635292](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110635292.png)

<br>

### 올리디버거

![image-20220404110712375](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110712375.png)

- 패킹 X

<br>

### F9으로 실행

![image-20220404110834825](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110834825.png)

- 아무것도 진행되지 않는 모습
- 어딘가 문제가 있다고 생각하고 성공 문자열로 들어가 보자

<br>

### All References text strings

![image-20220404110853683](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110853683.png)

- 성공 문자열 더블 클릭

<br>

![image-20220404110925727](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110925727.png)

- Message Box 함수 군집 코드 발견
- 어디서 호출하는지 찾아보자

<br>

![image-20220404110955830](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404110955830.png)

- 0040119B에서 메시지 박스 호출하는 것 발견
  - 윗줄에있는 PUSH 들은 메시지 박스에 들어가기위한 파라미터 값
- 00401188과 0040118A에서 CMP로 비교후 JNZ를 실행하는 모습 확인
- JNZ는 ZF값이 0일 때 004011A3 주소로 점프하는데 이렇게 되면 메시지박스 호출 못함
  - 즉, CMP에서 반환하는 ZF 값이 0이기 때문에 여기서 점프를 하게되므로 F9 실행 시 메시지박스 호출 못하고 갇혀있었던 것
- 점프를 못하게 만들어보자

<br>

### 방법 1

![image-20220404111729631](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404111729631.png)

- CMP에서 반환하는 ZF값이 0이므로 JNZ를 JE로 수정하여 실행해보자
  - JE는 ZF가 1일 때 점프

![image-20220404111757024](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404111757024.png)

- 성공적으로 메시지 박스를 띄우지만 문제에서 요구했던 CodeEngn은 담지 못함
- CMP에서 AL 값을 바꿔보자

<br>

### 방법 2

![image-20220404111847195](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404111847195.png)

- 해당 분기(JNZ) 부분의 00401188의 CMP 함수에 브레이크를 걸고
- POP으로 뽑은 EAX값의 AL(오른쪽 끝에서부터 1바이트(8Byte) 값) 값을 찾아보자
- AL = 0E(Hex) = 14(Dec) 이므로 1과 비교하면 거짓 값으로써 ZF 값 0 반환
  - ZF 값은 비교하는 두 대상이 같을 때 1 반환
  - 고급언어와 달리 어셈블리는 컴퓨터가 인식하는 것으로써 두 대상의 같은 것을 인지 못함
  - 그렇기에 두 대상을 마이너스 연산하여 그 연산결과가 0일 때 ZF를 1로 반환
  - SUB와 똑같이 마이너스 연산하지만 SUB는 연산결과를 저장하는 반면 ZF는 저장하지 않음(단지, 수단으로 사용)
- 그렇다면 AL과 1을 같게 해줌으로써 ZF를 1로 만들고 JNZ에서 점프하지 못하게하자
  - AL의 출처인 EAX를 어디서 PUSH하여 401187에서 POP 하는지 알아보자

<br>

![image-20220404112747319](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404112747319.png)

- EAX값을 POP 하기 전의 ESP(스택포인트)의 스택주소를 살펴보면 0040210E가 들어있는 것 확인
- 해당 0040210E가 PUSH 됐으므로 PUSH 0040210E 함수를 피해주면 AL값을 다른 값으로 조정 가능
- PUSH 0040210E를 하는 주소 찾아보자

<br>

![image-20220404112920444](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404112920444.png)

- 처음부터 확인해본 결과 00401037에서 PUSH 0040210E 확인 가능
- 여기서 또한 분기를 발견할 수 있는데 실행해보면 CMP에서 반환하는 ZF가 1이기 때문에 점프하지 못하고 PUSH 0040210E 수행
- EAX를 수정하여 ZF 값을 0으로 만들어 JNZ에서 점프를 시켜보자

<br>

![image-20220404113119046](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113119046.png)

- 수정하기 전 EAX 값을 확인해 본 결과 FFFFFFF(-1) 확인
- FFFFFFFF(-1)은 오류를 반환했다고 볼 수 있는데
- 윗줄에서 시행된 CALL 함수가 옳게 작동하지 않은 걸로 판단
- CreateFileA가 작동하지 않아 CRACKME3.KEY라는 이름의 파일이 생성되지 않음
- CRACKME3.KEY를 생성해주자

<br>

![image-20220404113232703](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113232703.png)

- 메모장을 열어서 다음과 같이 20.exe 파일이 있는 곳에 저장해 주면되는데
- 텍스트 문서가아닌 모든파일로 형식을 지정하여 KEY 파일을 생성할 것을 명심하자

<br>

### 올리디버거 재실행

![image-20220404113324707](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113324707.png)

- 같은 곳에 BP를 잡고 실행을 해보면 FFFFFFFF가 아닌 000000E4라는 값이 적재되어
- ZF가 0으로써 JNZ가 수행(점프)하게 됨

<br>

### F8로 차근차근 진행

![image-20220404113356895](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113356895.png)

- 진행하다보니 분기를 발견했고 분기되는 곳은 바로 앞서 넘어왔던 PUSH 0040210E
- 이 또한 넘기자
- CALL 함수의 반환값인 004021A0의 4Byte 값과 12(Hex)를 비교하는데
- 004021A0의 값은 0이고, 00401054에서 PUSH
- 00401054를 살펴보니 pBytesRead를 실행
  - pBytesRead는 해당 파일의 바이트 수만큼 읽어오는 함수이므로
  - CRACKME.KEY의 바이트 값을 004021A0에 적재하는 역할
- 12(Hex)인 18바이트 값을 맞춰주기 위해 CRACKME3.KEY로 이동하자

<br>

### CRACKME3.KEY

![image-20220404113743689](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113743689.png)

- 수정 후 올리디버거 재실행

<br>

![image-20220404113801597](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113801597.png)

- 값이 같아진 것 확인하므로 ZF 1 반환
- JNZ 점프 X

<br>

### F8 진행

![image-20220404113835000](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404113835000.png)

- CMP 함수와 JE 분기, 그리고 PUSH 0040210E 발견
- 게다가 00401037(PUSH 00402010E)로 분기하는 점프 함수
- 하지만 004010A1 주소의 PUSH 00402010E는 스택포인트에 4를 덧셈 연산(004010AB)해줌으로써
- 스택 자체를 옮겨버리기 때문에 POP할때  다른 값으로 인식
- 즉, JE 분기를 하지말아야 함 : ZF = 0
- ZF가 0이 되게 하기 위해선 TEST AL, AL이 0이 되면 안되는데
  - TEST AL, AL 은 비교 대상을 AND 연산하여 결과적으로 AL이 0인지 물어보는 것
    - TEST 함수는 결과값이 0이면 ZF = 1
    - 결과값이 0이 아니면 ZF = 0
- TEST AL, AL 이 0이 아니라면 AL이 0이 아니면 됨
- SETE AL 이 0으로 반환하게 하지 않으면 되는데
  - SETE 함수는 ZF가 1(세팅)이면 1이고, 0이면 0
- 즉, CMP EAX, DWORD PTR DS : [4020F9]의 ZF 값이 1이어야 함
- EAX와 4020F9의 4바이트를 같게 해주자

<br>

### Follow in Dump(00401093)

![image-20220404114127088](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114127088.png)

- EAX 값은 31313131의 아스키 값 = 1111 로써 CRACKME3.KEY에 입력한 데이터임을 확인
- 삽질로 몇변째의 데이터인지 확인해보니 마지막 4글자를 가져옴
  - 처음부터 식별가능한 문자로 했으면 편했을지도
- 자 그러면 4020F9의 값을 CRACKME3.KEY 마지막 4글자에 삽입

<br>

### HexEdit

![image-20220404114305871](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114305871.png)

- 다시 올리디버거 재실행

<br>

![image-20220404114321051](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114321051.png)

- 인텔 체제는 리틀엔디안 방식이기 때문에 거꾸로 적재
- 값 동일하므로 F9 눌러서 메시지박스 확인해보자

<br>

F9

![image-20220404114340745](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114340745.png)

- 이런씨발
- CodeEngn!이 아니라 다른거임
- 느낌표는 뽑혀있음
- 메시지 박스 함수 쪽으로 가보자

<br>

### Message Box

![image-20220404114406557](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114406557.png)

- 진행하다보니 덤프에서 MessageBox에 출력된 문자열들 확인가능
- ESI의 402008의 1Byte를 EDI 1Byte에 적재 확인
- 다시 시작하여 어디에서 현재 보이는 덤프 값들이 나오는지 확인해보자

<br>

### 00402008 ~ 00402018 Dump

![image-20220404114501036](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114501036.png)

- 확인 결과 CALL 20.401311을 호출한 뒤에 적재되는 걸로 확인
- F7으로 CALL 20.401311 함수로 들어가보자

<br>

### CALL 20.401311 - F7

![image-20220404114527621](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404114527621.png)

- 해당 알고리즘 발견
- 해당 알고리즘에서 메시지 박스 값 도출하는 걸로 확인 가능
- ECX, EAX를 0으로 초기화
- ESP+4(CRACKME3.KEY에 들어있는 값)을 ESI에 적재
- BL = 41
- ESI의 한 바이트를 AL에 적재
  - CRACKME.KEY 값으로부터 도출해낸 ESI 스택 하나 값을 AL에 적재
- AL과 BL XOR 연산
- AL값을 ESI의 한 바이트에 적재
  - 빠져나간 ESI의 스택 공간에 XOR 한 AL 값 적재
- ESI와 BL 1씩 증가
  - ESI의 스택 칸 올려줌(XOR 연산 끝난 문자 다음 문자를 XOR하기 위해)
- EAX 값을 4020F9 4바이트에 적재
  - CRACKME3.KEY의 마지막 4글자 / 즉, 키값 생성이라고 보면 됨
  - 수정할 때마다 4020F9값을 확인해서 같이 수정해주자
- AL과 0 비교해서 AL이 0이면 알고리즘 종료
- AL이 0이 아니면 CL 증가
  - CL 갯수 만큼 문자열 출력
- BL, 4F 비교하여 다르면 처음 알고리즘으로
  - BL은 0x41~0x4F 까지 반복
- 이를 파이썬 코드로 짜보자

<br>

### 파이썬

![image-20220404115041102](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404115041102.png)

![image-20220404115050788](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404115050788.png)

![Untitled](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404115102787.png)

- 헥스 에딧으로 수정해주자

- 알고리즘에 의해 수정된 4020F9값 확인해서 같이 수정

  ![image-20220404115142106](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404115142106.png)

<br>

### 다시 올리디버거로 실행(F9)

![image-20220404140639211](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404140639211.png)

- 성공적으로 CodeEngn 문자열이 출력되었는데 필요없는 문자열도 출력된 거 확인
- 이는 알고리즘에서 BL값이 0x41~0x4F 까지 반복된만큼 CL값이 같이 증가되어서
- CL 값만큼 문자열을 출력하기 때문에 모두 출력된 현상
- 그렇다면 Offset(h)의 08번 0x49에서 종료를 시켜야하는데
- 알고리즘에서 AL과 0을 비교해서 AL이 0이면 종료하는 구문을 이용해보자

<br>

### HexEdit

![image-20220404140813156](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404140813156.png)

- BL이 49일 때 종료가 되어야하므로 AL과 BL이 XOR 연산하여 0이되려면

- AL 또한 아스키 값이 49이어야 함

- 그러므로 HexEdit에서 Offset(08)를 49로 수정해주자

- 마찬가지로 4020F9값도 같이 수정

  ![image-20220404140840842](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404140840842.png)

<br>

### 결과 - F9

![image-20220404140902505](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220404140902505.png)

- 성공