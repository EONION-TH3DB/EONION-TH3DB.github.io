---
title: "[Dreamhack] War Game 1단계 - Reversing Basic Challenge #0"

---

<br>

## 문제

![image-20211211000517814](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211000517814.png)

<br>

<br>

## 프로그램

![image-20211211000839564](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211000839564.png)

- 문자열 입력하는 프로그램
- 입력시 correct나 wrong 출력

<br>

<br>

## 패킹

![image-20211211001016290](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211001016290.png)

- 패킹 되어 있지 않음

<br>

<br>

## x64dbg

### OEP

![image-20211211001427613](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211001427613.png)

<br>

### 실행(R)

![image-20211211001448689](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211001448689.png)

- 아직 input 입력칸 나오지 않음

<br>

### 실행(R)

![image-20211211002027483](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211002027483.png)

- 입력칸 등장
- 실행을 한 번 눌렀을 때 멈췄던 주소인 7FF7612E1564 부터 한단계씩(건너서 단계진행 버튼) 진행해보자

<br>

### 건너서 단계진행(O)

![image-20211211002118442](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211002118442.png)

- 7FF7612E14EF 에서 특정 구간 호출후에 Input 입력값 뜨는 것 확인
- BreakPoint로 잡고 처음부터 실행하고
- 안으로 들어가보자

<br>

### 안으로 단계진행(I)

![image-20211211002631220](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211002631220.png)

- 호출된 집단은 이렇게 생김
- 분기점을 발견할 수 있는데 eax가 0인지 아닌지 검사
  - 0이면 Wrong / 0이 아니면 Correct
  - eax가 마지막으로 어디에쓰였는지 찾아보자
- 분기 바로 위의 호출하는(CALL) 곳이 test eax,eax와 분기(je)를 실행하기 위한 인자들이 생성되는 곳으로 판단할 수 있음, 즉 eax를 마지막으로 사용한 곳으로 판단

- CALL 되는 구간이 어떤 기능을 하는지 한단계씩 진행하여 파악해보자

<br>

### 건너서 단계진행(O)

![image-20211211003038573](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211003038573.png)

- 1번은 Input 값 출력하는 함수
- 2번은 write 할 수 있게 입력값 저장하는 함수로 추정이 된다
- 3번은 위에도 추정했다싶이 입력된 값과 correct 문자열을 cmpare하는 곳으로 판단된다.
- 3번으로 들어가보자

<br>

### 안으로 단계진행(I)

![image-20211211004734549](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211004734549.png)

- 여기서 또한 test eax,eax를 통해 eax값이 0인지를 묻는 jne 분기 발견
  - eax가 0이면 rsp+20에 0값이 저장되고
  - eax가 0이아니면 rsp+20에 1값이 저장된다.
- 이 구문을 빠져나갈 때 eax가 1이어야 Correct가 되므로 test eax,eax에서 eax값은 0이되어야 한다.
  - 그래야 jne 구문을 통과할 떄 rsp+20에 1값이 저장되기 때문이다.
  - 그러고 마지막 mov 함수로 rsp+20의 값이 eax에 담겨 최종적으로 eax를 1로 반환한다.
- 그렇다면 <JMP .&strcmp> 함수를 알아보자
  - <JMP .&strcmp> 함수는 두 문자열을 비교해서 동일하면 0 / 다르면 1을 eax에 저장
- 그렇기 때문에 위의 두 인자인 rdx에 입력된 문자열과 rcx에 입력된 문자열을 비교한다.
  - rdx는 7FF7612E2220의 주소가 저장되는데 Compar3_the_str1ng 값이 입력되고
  - rcx는 우리가 입력한 문자열 값이다.
- 이 둘을 비교하면 되기 때문에 정답은 'Compr3_the_str1ng' 이다.

<br>

### 결론

![image-20211211010146201](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20211211010146201.png)
