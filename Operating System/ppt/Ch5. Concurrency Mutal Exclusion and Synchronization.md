# Ch5. Concurrency : Mutal Exclusion and Synchronization

## 1. Multi Processes

프로세스와 쓰레드들을 어떻게 관리할건가?!?

#### 1. Multiprogramming

> 단일 프로세서 시스템 내의 다중 프로세스들의 관리 (실행 순서, 자원 공유관리)

#### 2. Multiprocessing

> 다중 프로세서 시스템 내의 다중 프로세스들의 관리.

#### 3. Distributed Processing

> 다중 분산 컴퓨터 시스템 내의 다중 프로세스의 관리.

<br/>

## 2. Key terms related to Concurrency

#### 1. atomic operation

> 원자, 더 이상 안나눠짐.  **atomic operation**이 보장이 안되면 데이터 무결성이 깨질 수 있음. 그래서 필수적인 요소이며 연산이 실행될 때 아예 실행이 안되거나 실행이 된다면 도중에 중단되지 않고 끝가지 실행을 마칠 수 있어야함.

#### 2. critical section

> 프로세스 내 코드 구역으로 공유 자원으로 접근이 요구되며 한 프로세스가 실행되고 있을시 다른 프로세스는 접근 할 수 없도록 해야함.

#### 3. deadlock

> 두개 혹은 그 이상 프로세스가 실행되지 못하고 대기하는 상황.

#### 4. livelock

> **deadlock**과는 다른 개념. 두개 혹은 그 이상 프로세스들이 다른 프로세스 변화에 반응하여 자신들의 상태를 변화하는것. 이는 의미없는 동작을 계속 하는 상태. (예를들어 A, B가 서로 활성 <-> 비활성 상태를 반복해서 변경하고 있는 상태면 실제론 의미있는 작업이 일어나고 있지 않음)

#### 5. mutual exclusion

> 상호 배제. **critical section이 만족해야하는 특성**으로 critical section에 있는 한 프로세스가 공유된 자원에 접근할 때 어떠한 critical section내 다른 프로세스도 공유된 자원에 접근할 수없다.

#### 6. race condition

> 공유된 자원을 다중 쓰레드 혹은 다중 프로세스가 읽고 쓰는 상황에서 최종 결과는 그들의 실행 순서에 따라 달라질 수 있다. 공유 자원, 데이터에 대해 여러 개의 프로세스, 혹은 쓰레드가 동시에 접근해 문제를 야기할 수 있는 상황을 **race condition**이라 한다.

#### 7. starvation

> 실행 가능한 프로세스가 스케줄러에 의해 방치되어 무한정 대기되는 상태.(예를 들자면 block된 상태에서 우선순위가 낮아 해당 프로세스보다 우선순위가 높은 프로세스들이 반복적으로 선택되면 실행되지 못하고 대기시간이 무한정 길어지게 된다.)



<br/>

## 3. Dekker's algorithm

- 두 프로세스간의 **상호배제**를 위해 만들어졌다. 만약 두 프로세스가 동시에 **critical section**에 접근을 시도한다면, 오로지 한 프로세스만 접근을 허용하는데 누구 차례인지에 기반하여 한다.

- 두 **flag**를 사용하여(flag[0], flag[1]) **critical section**에 접근 의사를 표현한다. 그리고 **turn**변수로 두 프로세스 간의 우선순위를 나타낸다.

<br/>

**Four attempts**

- First attempt

  - process 0

  ```c
  while(turn != 0) / waiting...
      / do nothing /
      / critical section /
      turn = 1; / 차례 넘기기
  ```

  - process 1

  ```c
  while(turn != 1) / waiting...
      / do nothing /
      / critical section /
      turn = 0; / 차례 넘기기
  ```

  **두 프로세스가 번갈아 실행. 만약 두 프로세스중 하나가 너무 느리다면 너무 영향을 많이 받음.**

  <br/>

- Second attempt

  - process 0

  ```c
  while(flag[1]) / waiting...
      / do nothing /
      flag[0] = true;
      / critical section /
      flag[0] = false;
  ```

  - process 1

  ```c
  while(flag[0]) / waiting...
      / do nothing /
      flag[1] = true;
      / critical section /
      flag[1] = false;
  ```

  **만약 두 flag가 false라면 critical section에 동시접근 하므로 mutual exclusion이 보장되지 않음**

  <br/>

- Third attempt

  - process 0

  ```c
  flag[0] = true;
  while(flag[1]) / waiting...
      / do nothing /
      / critical section /
      flag[0] = false;
  ```

  - process 1

  ```c
  flag[1] = true;
  while(flag[0]) / waiting...
      / do nothing /
      / critical section /
      flag[1] = false;
  ```

  **Deadlock 발생, 서로 기다려서 진행이 되지 않음!**

  <br/>

- Fourth attempt

  - process 0

  ```c
  flag[0] = true;
  while(flag[1]) / waiting...
      flag[0] = false;
      / delay /
      flag[0] = true;
  
  / critical section /
  flag[0] = false;
  ```

  - process 1

  ```c
  flag[1] = true;
  while(flag[0]) / waiting...
      flag[1] = false;
      / delay /
      flag[1] = true;
  
  / critical section /
  flag[1] = false;
  ```

  **Livelock 발생, 상태는 바뀌지만 결국 while문을 반복!**

<br/>

- 이전 코드들은 turn, 우선순위 부여가 없었음. **Dekker's algorithm **에서는 각 프로세스가 자신의 차례인지를 확인하는 조건이 추가되었다.

- **Peterson's algorithm **에서는 flag와 trun이라는 전역변수를 사용해서 3가지 조건(상호 배제, 진행, 한정 대기)를 만족시킨다.

<br/>

## 4. Requirements for Mutual Exclusion

**다음 6가지 조건을 살펴보자.**

- 상호 배제는 반드시 요구됨! 오직 하나 프로세스가 **critical section** 에 허용된다. 모든 프로세스가 **critical section** 에 같은 자원을 요구하고 공유하고 싶더라도!
- **critical section** 가 아닌곳에서는 다른 프로세스에 영향을 받지 않고 중단가능.
- **critical section** 에 아무도 없고 프로세스가 원하면 바로 접근 가능해야한다. *Deadlock*, *Starvation* X
- **critical section** 에 프로세스가 요청할 때 지연되어선 안된다.
- 프로세스 개수나 속도 관계에 대한 가정은 없다.
- 프로세스는 **critical section** 에 유한한 시간만 머물 수 있다.

<br/>

**mutual exclusion 을 지원하는 HW 기능**

- Interrupt Disabling: interleaved 할 수 있도록 프로세스는 OS 가 회수하거나 인터럽트 발생
전까지는 실행할 수 있음. 근데 이거는 멀티프로세서 환경에서는 작동하지 않음.
- Compare & Swap Instruction: 비동기화를 위해 멀티쓰레딩 프로그래밍에서 사용하는 atomic
instruction 이다. 메모리 주소값이 주어진값과 동일하다면 해당 메모리 주소를 새로운 값으로
대체한다. 간단한 bool 을 반환하기도 하고 메모리에서 읽은 값을 반환하기도 한다.

<br/>

## 5. Semaphore

정수형 변수로 오직 세 함수에 의해서 정의된다. 초기화, semWait, semSignal 세 함수를 기억하자.

- semWait : 프로세스가 **critical section**에 진입 전에 호출하는 함수.
- senSignal : 프로세스가 **critical section**에서 나올 때 호출하는 함수.

<br/>

**Strong semaphore** vs **Weak semaphore**

- **Strong semaphore**는 큐에서 FIFO 규칙에 따라 프로세스가 제거되는 방면 **Weak semaphore**은 큐에서 프로세스가 제거되는 규칙이 명시가 되어있지 않다.

<br/>

**Mutex** vs **semaphore**

- **mutex**는 이진 세마포어와 유사하다. 하지만 mutex에서는 프로세스가 소유권을 가져서 **critical section**에 진입하고 나오는 프로세스가 직접 세마포어 변수를 증감시킨다.

<br/>

## 6. Producer / Consumer Problem

- 하나혹은 그 이상의 producer가 데이터를 만들고 버퍼에 위치시킵니다.
- 단일 consumer가 한번에 하나씩 버퍼에 있는 데이터를 가져갑니다.
- 오직 한 producer 나 consumer만이 버퍼에 접근할 수 있습니다.
- 여기서 **주의 해야할 점**은 producer은 버퍼가 가득 찼을 때 데이터를 추가하면 안되고, consumer은 버퍼가 비었을 때 데이터를 가져가려 하면 안됩니다.

<br/>

## 7. Message Passing

- 프로세스가 다른 프로세스와 상호 작용하려면 **Synchronization**과 **communication**을 만족해야한다. **Message passing**은 두 기능을 제공할 수 있는 한가지 접근법이다.
- A와 B라는 프로세스가 있다고 가정하자.
  - Direct : A가 커널에 메시지를 주고 커널이 B에게 메시지를 전달한다.
  - Indirect : A가 커널에 메시지 박스를 두고 B에게 가져가라고 얘기한다. 그러면 B는 커널에 메시지박스에 접근해 읽어온다.

<br/>

## 8. Readers / Writers Problem

- Readers는 데이터를 읽기만 하고 Writers는 데이터를 쓰기만 한다.
- 여기서 주의할 점은 오직 한 Writer만 데이터를 쓸 수 있고 다른 Writer은 쓰지 못한다. Reader는 다른 Reader가 있어도 상관이 없다.