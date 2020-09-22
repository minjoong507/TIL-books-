# Ch3. Process Description and Control

## 1. What is Process?

#### 1) Definition

이전 단원에서 여러가지로 정의하였다. 간략하게 두가지로 얘기하자면

- program code
- A set of data associated with that code



<br/>

#### 2) Process Elements

1. Identifier
2. State
3. Priority
4. Program counter - 현재 수행되는 위치
5. Memory pointers
6. Context data
7. I/O status information
8. Accounting information

<br/>



#### 3) PCB (Process Control Block)

- 앞서 프로세스 요소를 포함한다.
- OS에 의해 생성, 관리된다.
- 멀티 프로세스 지원을 허용하는 핵심적인 툴이다.

<br/>

## 2. Two-State Process Model

<div>
<center>
<img src="https://github.com/minjoong507/T.I.L/blob/master/Operating%20System/img/two-state-process-model.png" width="60%;"></img>
</center>
</div>


- 프로세스 상태는 Not running, Running 두가지로 존재.
- 디스패처가 프로세스 상태를 바꿔줌. Ready queue에 존재하는 프로세스를 선택.

<br/>

## 3. Five-State Process Model

<div>
<center>
<img src="https://github.com/minjoong507/T.I.L/blob/master/Operating%20System/img/five-state-process-model.jpg" width="60%;"></img>
</center>
</div>


- Two-State와 다른점은 제목 그대로 세가지 상태가 더 많이 있음.
- States
  * New : 주 기억장치에 없음. 생성, 코드가 메모리로 들어오지 않은 상태.
  * Ready : 기회가 주어지면 수행할 준비가 되어있는 프로세스
  * Running : 실행중인 프로세스
  * Exit : 실행이 종료된 프로세스
  * Blocked : 입출력 연산 완료와 같은 이벤트가 발생할 때 까지 실행되지 않고 기다리는 프로세스
- 여기서 문제는 Blocked state process는 실행되지 않지만 메모리는 차지하고 있음. 얘를 어떻게 처리해야 더 효율적으로 할 수 있을까?

<br/>

## 4. Suspended Processes

<div>
<center>
<img src="https://github.com/minjoong507/T.I.L/blob/master/Operating%20System/img/suspend-state-process-model.png"></img>
</center>
</div>


- Swapping : 프로세스 일부나 전체를 메인 메모리로 부터 디스크로 이동하는 방법이다.
- Blocked와 차이는 프로세스가 메인 메모리에 존재하는지에 대한 여부.
- Suspended Process의 특징 4가지
  * 즉각적으로 실행 불가능
  * 실행 방지를 목적으로 스스로, OS 혹은 사용자로 부터 suspended됨.
  * 이 프로세스가 이벤트를 기다리는 안기다리는지 모름.
  * 사용자가 명시적으로 상태를 바꿔주지 않으면 프로세스는 삭제가 안될 수 있다.

<br/>

## 5. 