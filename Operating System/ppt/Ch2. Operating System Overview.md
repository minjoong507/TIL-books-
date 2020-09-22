# Ch2. Operating System Overview

## 1. Serial Processing

- 초기에는 OS가 없었음. 프로그래머들은 컴퓨터 HW와 직접적으로 반응해야했음.

- 이는 굉장히 비효율적 왜?

  > 1. Scheduling
  >
  >    컴퓨터의 작동시간을 낭비하게됨.
  >
  > 2. Setup time
  >
  >    프로그램을 설치하고 실행하는데 굉장히 긴 시간이 필요로 했음.

<br/>



## 2. User Mode, Kernel Mode

#### 1) User Mode

- 유저 모드에선 유저 프로그램이 동작
- 특정 메모리 영역은 사용자 접근으로 부터 보호되어야함.
- 특정 명령어는 실행하지 못함.
- HW에 직접적인 접근 금지. System call을 통해 Kernel Mode로 전환.

<br/>

#### 2) Kernel Mode

- 커널 모드에서 모니터가 실행.
- 권한을 가진 명령어가 실행된다.
- 보호가 필요한 메모리 영역에 접근이 가능하다.

<br/>

#### 3) User Mode vs Kernel Mode

- 왜 mode를 두개로 나눴을까?

  >  커널은 일반 사람들이 다루기 힘든 자료가 있음. 사용자 모드에서 접근이 불가능한 영역도 존재. 그땐 Mode switching을 통해서 접근.
  >
  >  또한 I/O 장치를 보호할 수 있음. app program은 오로지 OS를 통해서만 I/O에 접근 가능함.

<br/>

## 3. Uniprogramming

- 프로세서가 I/O 명령어에 도착할때 까지 특정 실행 시간을 소비함. 그러면 I/O 명령어 시간까지 포함하여 기다리게 되는것.
- 이는 대기시간이 매우 길어짐.

<br/>

## 4. Multiprogramming

- Uniprogramming 보다 효율적.
- 프로그램을 돌아가면서 실행하기 때문에 무작정 I/O을 위해 기다릴 필요가 없음.
- CPU의 대기시간을 줄일 수 있는 장점이 있다.

<br/>

## 5. Time Sharing Systems

- 시분할 시스템, 대화형 멀티 프로그래밍
- 다중 사용자들이 프로세서의 시간을 공유함. 그래서 여러명이서 쓰는거 같지만 혼자 쓰는 것 같이 해줌.
- 일정 Time slice만큼 cpu에 할당하고 그 시간이 끝나면 상대방에게 넘어감. 짧은 시간동안 서로 번갈아 사용함.
- Multiprogramming과는 목표가 다름. Multiprogramming는 프로세서 사용 시간의 극대화 했다면 time sharing은 응답시간을 최소화 하는 것이다. 이 차이를 이해하자!

<br/>

## 6. Major Achievements

OS가 지속적으로 해결, 발전해나가야할 과제들.

- Processes
- Memory management
- Information protection and security
- Scheduling and resource management
- Different OS architectural approaches

<br/>

## 7. Process

프로세스의 여러 정의를 살펴보자.

- 수행중인 프로그램
- 실행중인 프로그램의 인스턴스
- 프로세서에 할당, 실행될 수 있는 모든것.
- 시스템 자원의 집합, 현 상태 등의 활동 단위.

<br/>

## 8. Causes of Errors

뒤에 다룰 내용이지만 간략하게 확인하고 가자.

#### 1) Improper synchronization 

- 부적절한 동기화
- 누굴 기다리다 실행이 될텐데 신호가 적절치 않다면 분실, 중복전달 발생!

#### 2) Failed mutual exclusion

- 상호 배제실패
- 공유자원 사용을 시도하는 경우가 있음. 이때, 제어가 제대로 안된다면 에러 발생.

#### 3) Nondeterminate program operation

- 비결정적 프로그램 연산
- 프로그램 스케줄이 어떤 순서로 되었느냐에 따라 어느 특정 프로그램에 영향을 끼칠 수 있다.

#### 4) Deadlocks

- 데드락, 교착상태
- 두개 혹은 그 이상 프로그램이 어떠한 이유로 서로가 끝나길 기다리는 상태. 데드락이 발생하는 조건, 이유는 뒤에 더 자세히 다루자.

<br/>

## 9. Microkernel Architecture

- 핵심 기능만 커널에 포함시키자.
- 이를 통해 유연성을 제공, 구현 간편화, OS의 휴대성을 향상시킬 수 있음.

<br />

## 10. Interleaving and Overlapping

이 둘의 차이는 뭘까?

- Interleaving은 프로세스를 번갈아 여러개를 실행시킬 수 있지만 물리적으로 동시에 실행시키진 못함.
- Overlapping은 프로세서가 여러개로 물리적으로 동시에 실행이 가능하다.