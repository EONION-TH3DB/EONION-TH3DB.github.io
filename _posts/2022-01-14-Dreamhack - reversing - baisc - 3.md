---
title: "[Dreamhack] War Game 1단계 - Reversing Basic Challenge #2"

---

<br>

## 문제

![image-20220114234110204](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220114234110204.png)

<br><br>

## 문제 파일

![image-20220113223836765](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220113223836765.png)

<br><br>

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

여기까지는 rev - basic - 2 랑 같다.

<br><br>

### 3번 Call 내부

<img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220114234607151.png" alt="image-20220114234607151" style="zoom:107%;" />

- Correct가 나오기 위해서는 마지막에서 3번째 주소인 mov eax, 1 을 거쳐야 한다.

- 그러기 위해선 4B 주소인 je chall 3.7FF81BB1051 코드를 참으로 거쳐야 하는데

- 처음 실행을하여 10번 비교에서 참이나오면 반복문을 진행하게 된다.

  - 자세한 설명은 rev - basic - 2 를 참고바란다.
  - rev - basic - 2 문제와 반복문(1~10번)만 다름

- 그렇다면  1~10번 코드를 해석해보자.

  - rsp = counter(cnt)

  - rsp + 20 = 내가 입력한 문장(str)

  - 7FF781BB3000 = 정답(cor)

    <img src="https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220115000636043.png" alt="image-20220115000636043" style="zoom:150%;" />

- 1 : rax에 cnt 갱신 >> rax = cnt

- 2 : rcx에 cor 주소 전달

- 3 : eax에 정답의 문자열 카운트 갱신 >> eax = cor + cnt

- 4 : rcx에 cnt 갱신 >> rcx = cnt

- 5 : rdx에 str 갱신 >> rdx = str

- 6 : ecx에 내가 입력한 문자열 카운트 갱신 >> ecx = str + cnt

- 7 : ecx와 rsp를 xor 연산 >> ecx = [str + cnt] ^ cnt

- 8 : edx에 cnt 갱신 >> edx = cnt

- 9 : ecx에 [ecx + cnt * 2] 주소 전달 >> [ecx + cnt * 2] = [(str + cnt) ^ cnt] + cnt * 2

  - 질문 rcx가 왜 ecx가 되는 매직이있는가

- 10 : 정답 관점에서 보면 eax = ecx 가 되어야함
  - cor + cnt  = [(str + cnt) ^ cnt] + cnt * 2
  - 정답 = [(내가 입력한 값) ^ cnt] + cnt * 2
  - 내가 입력한 값 = [정답 - cnt * 2] ^ cnt
  - 정답(16진수 라인)을 알고 있기 때문에 내가 입력한 값을 정답으로 도출하기 위해 역연산을 통해 답을 구해보자

<br><br>

## Python

![image-20220115002010918](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220115002010918.png)



## 결과

![image-20220115002030096](https://raw.githubusercontent.com/EONION-TH3DB/image_repo/main/img/image-20220115002030096.png)