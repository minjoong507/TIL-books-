# Ch6. Concurrency Deadlock and Starvation

## 1. Principles of Deadlock

- 먼저 데드락의 정의부터 살펴보자면 데드락은 경쟁관계의 프로세스들의 진행이 영구적으로 막히는 상황을 의미한다.
- 왜 영구적이냐면 어떠한 이벤트도 일어날 수 없기 때문이다.
- 데드락의 예시
  - 1. 자원에 상호 접근
  - 2. 메모리 요구, 예를 들어 200Mb용량 제한이 있고 P1이 80Mb, P2가 80Mb를 요구했다고 하자. 그리고 P1과 P2가 50Mb씩 더 요구했다고 가정하면 남은 용량은 40(200-160)Mb밖에 없으르모 둘다 대기할 수 밖에 없음
  - 3. 설계의 오류
  - 4. 서로 소유한 자원에 대해서 요구하는 경우

<br>

## 2. The conditions for deadlock

 데드락이 발생할 수 있는 네가지 조건을 살펴보자. 1, 2, 3번 조건은 필요조건, 4번은 충분조건이다. 확인해보자.

#### 1.  Mutual Exclusion

> 오직 한 프로세스만이 자원을 사용할 수 있다. 다른 프로세스는 할당받지 못한다.

#### 2. Hold - and -Wait

>  자원을 가지고 있는 상태에서 또 다른 자원을 요구하는 경우. 이미 누가 점유하고 있는 자원을 요구하는 경우면 deadlock이 발생.

#### 3. No Pre-emption

> 프로세스가 가지고 있는 어떠한 자원도 강제적으로 제거될 수 없다.

### 4. Circular Wait

>  서로 가지고 있는 자원을 요구해서 사슬 형태를 이루고 있는 경우를 말한다. P1, P2, P3가 서로의 자원을 기다리는 상황을 생각해보자.

<br/>

## 3. Deadlock approaches

어떻게 하면 데드락을 방지할 수 있을까? 세가지 보편적인 방법이 있다.

#### 1. Deadlock prevention

>  데드락을 원천적으로 발생할 수 없게 만들어 버리면 되지 않을까? Indirect, direct 방법.
>
> - Indirect : 위의 필요조건 3가지 중 하나를 제거.
> - direct : circular wait을 제거.

![deadlock prevention](https://github.com/minjoong507/T.I.L/blob/master/Operating%20System/img/deadlock%20prevention.PNG)

<br/>

#### 2. Deadlock avoidance

>  필요조건 3가지를 고려하여 데드락 발생 가능성을 확인한 다음 자원을 할당해주자. 근데 사실상 불가능한게 어느 자원을 요청할지 미리 알고 있어야함. 두 가지 접근법을 살펴보자.
>
> - Resource Allocation Denial : 만약 프로세스의 단계적 자원 요청이 데드락이 발생할 가능성이 있다면 할당하지 않음. [*banker's algorithm*](https://jhnyang.tistory.com/102)
> - Process Initiation Denial : 애초에 데드락이 발생할 것 같은 프로세스는 시작하지 않음.

<br/>

#### 3. Deadlock detection

>  일단 가능한 대로 자원을 할당해주는데 데드락을 확인하여 추가적인 작업. 
>
>  자원 할당이 가능하다면 프로세스에게 할당해줌. OS는 circular wait 조건을 탐지하기 위한 알고리즘을 실행. 장점으로는 데드락 탐지를 빠르게 할 수 있고 알고리즘이 상대적으로 단순하다. 하지만 단점은 매번 알고리즘을 실행시키면서 확인해야 하기 때문에 프로세서 시간이 상당히 소요된다.
>
>  **여기서 Deadlock avoidance와의 차이? Deadlock avoidance는 데드락이 발생하는지 미리 검사하여 발생한다면 자원을 할당해 줄 수 있음에도 불구하고 할당해주지 않음. 하지만 Deadlock detection은 할당해줌.**

<br/>

## 3. Dining Philosophers Problem

- 두 철학자가 동시에 같은 포크를 사용하지 못함, **mutual exclusion**
- 어느 철학자도 deadlock 이나 starvation되어선 안됨.

**만약 모든 철학자가 동시에 왼쪽 포크를 들어버리면 모두가 서로 포크를 놓기까지 기다려야함. Deadlock 발생!**

- 이를 해결하기 위해 5명의 철학자가 있다고 가정하고 dining room을 만들어 4명까지 들어올 수 있게 만듬. 그렇게 하여 4명의 철학자와 5개 포크가 있으니 한 철학자는 무조건 포크 두개를 사용할 수 있음!
- 위의 방법으로 deadlock 과 starvation 해결.

