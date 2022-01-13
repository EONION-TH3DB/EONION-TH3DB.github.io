---
title: "[Dreamhack] War Game 1단계 - Reversing Basic Challenge #2"
---

<br>

## 문제

![image-20220113223708890](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113223708890.png)

<br>

<br>

## 문제 파일

![image-20220113223836765](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113223836765.png)

<br>

<br>

##  x64dbg

### 시작화면

![image-20220113223937445](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113223937445.png)

<br>

### 문자열 참조

![image-20220113224220887](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113224220887.png)

<br>

### Correct 찾기

![image-20220113224330730](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113224330730.png)

- 더블 클릭(이동)

<br>

### Call 명령어에 Break

![image-20220113224655739](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113224655739.png)

- 먼저 Correct를 출력하기 위해서는 어떤과정을 거쳐야하는지 Correct의 분기인 7FF662941175를 확인해보자

  test eax, eax

  je chall2.7FF662941186 -> 참일시에 Wrong 출력하는 주소로 이동

  - eax값이 0인지 확인하여 eax값이 0일 때 참
    - (AND 연산에서 결과값이 0이기 위해 eax값은 0이어야 함)
  - 즉, eax 값이 0이 아니어야 Correct를 출력

- 그러기 위해서는 3번 Call에서 넘겨받은 eax가 0이 아니어야 함

  - 3번 Call에서 우리가 입력한 문자열과 정답인 문자열을 비교하는 구문 있을거라 판단

- 3번 Call을 알아보기 전에 각 Call 명령어가 무슨 역할 하는지 알아보자

<br>

### 1번 Call

![image-20220113224754410](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113224754410.png)

- 1번 Call에서 실행을 눌러 다음 브레이크에서 멈췄을 때 'nput : ' 이 출력되는 것 확인
- 문자가 입력되지는 않음

<br>

### 2번 Call

![image-20220113224917863](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113224917863.png)

- 2번 Call을 진행했을 때 문자 입력되는 것 확인
- 2번 Call 밑에 rcx에 rsp+20의 주소를 집어 넣는 것으로 보아 rsp+20이 우리가 입력한 특정 문자열로 추측이되므로 확인해보자

### 2번 Call 내부

![image-20220113225148514](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113225148514.png)

- 순차대로 진행했을 때 2번 Call 내부의 1번 call에서 문자 입력되는 것 확인
- 2-1 call 내부의 반환값인 eax 값을 rsp+20에 넣어주는 것 확인했으므로 
- rsp+20 = 우리가 입력한 문자열

<br>

### 3번 Call 내부

![image-20220113230919286](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113230919286.png)

- 먼저 1번 분기를 확인해보자
  - rax와 12를 비교하여 jae 점프 명령어를 실행
    - rax가 12보다 크거나 같으면 참으로 Jump
  - mov eax,1 로서 eax가 0이 되지 않기 위해서는 1번을 무조건 거쳐야 함
- 2번은 절대로 거치면 안된다.
  - xor eax,eax떄문에 eax값이 0이 돼버리기 때문
- 3번 분기를 확인해보면 ****46 주소로 이동후 ****12주소로 4번을 실행하여 위로 이동하는 모습이다.
  - 즉, 반복문이 이어질거라 예상할 수 있다.
- 5번은 3, 4번 반복문이 있기전의 처음 실행시 이동하는 주소로 판단된다.

### 3번 Call 내부 - 실행(R)

![image-20220113230919286](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113230919286.png)

- 자 그러면 처음부터 코드해석을 해보며 진행해보자
- rcx(우리가 입력한 문자열)값을 rsp+8 값에 넣어주고
- rsp에 0값을 넣어준다. rsp = 0
- 그리고 5번 jmp하여 rsp 값을 rax에 넣어주고 rax = 0
- rax와 12를 cmp 하여 jae 분기(1번) 진행한다.
  - rax는 0이기 때문에 거짓으로 분기되지 않고 밑 주소로 이동
- rax값에 rsp 값 넣어주고 rax = 0
- rcx에 7FF662943000의 주소를 넣어준다.
  - 리버싱 입문자가 아니라면 여기서 어느정도 감이올 것이다.
- rsp 값을 rdx에 넣어주고 rdx = 0
- r8에다가 rsp+20(우리가 입력한 값)을 넣어준다.
- edx에 byte로 r8+rdx를 넣어준다.
  - '우리가 입력한 값 + 0' 이 되는데 byte 값으로 받기 때문에
  - 즉, 우리가 입력한 값의 첫번째 문자를 가져와서 edx에 저장한다.
- rcx+rax*4 와 edx를 비교하여 분기를 실행한다.
  - 답은 7FF662943000 값이라는 것을 알 수 있다.
  - 하지만, rcx는 7FF662943000이고 rax값은 0인데 여기에 곱하기 4를 진행한다.
  - 왜 그런지 7FF662943000 을 확인해보자

### 7FF662043000

![image-20220113232529980](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113232529980.png)

- 덤프로 따라가자

![image-20220113232632925](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113232632925.png)

- Comp4re_the_arr4y 인것을 확인할 수 있다.
- 하지만, 4byte로 띄어서 값을 저장한 것을 확인할 수 있다.
- 이는 3번 Call 내부에서 rcx + rax * 4에서 
  - rax가 0이면 rax * 4는 0으로 rcx 문자열의 첫번째 문자인 'C'를 갖고오게 하기 위함이고 
  - rax가 1이면 rax * 4는 4로서 rcx 문자열의 두번째 문자인 'o'를 가져오게 하기 위함이다.

<br>

### 3번 Call 내부

![image-20220113230919286](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113230919286.png)

- 다시 돌아와서
- rcx+rax*4 와 edx를 비교하는 것은 우리가 입력한 문자열 한 글자와 정답인 문자열 한 글자를 비교하여 3번분기를 실행한다.
- 3번 분기가 실행된 후에 4번 jmp가 실행되어 계속해서 반복하게 되는데
- 4번분기가 실행된 후 rsp 값을 eax값에 집어넣고 rsp = 0, eax = 0
- eax 값을 1 증가 eax = 1
- 다시 eax값을 rsp에 넣어주고 지금부터는 처음부터 반복된다.
  - 즉, rax값과 rdx값을 주목해주면 되는데 eax값이 증가함에 따라 이 두값이 같이 증가하게 되므로
  - 증가한 값의 문자열 칸 값들이 비교되어 반복문이 진행되고
  - 마지막 반복문의 1번 분기에서 rax가 12보다 크거나 같을 때 1번 분기가 참으로 jmp되어 eax에 1을 반환하고 종료된다.
    - 여기서 주의해야할 점이 cmp하는 값 12는 10진수가 아닌 Hex(16진수)이다.
    - 12(16진수) = 17(10진수)
  - 즉, eax는 0이 아니므로 Correct가 출력된다.

<br>

<br>

## 정답

`Comp4re_the_arr4y`