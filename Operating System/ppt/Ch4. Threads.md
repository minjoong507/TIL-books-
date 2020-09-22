# Ch4. Threads
## 1. Processes and Threads
- Resource Ownership
  > 프로세스는 프로세스 이미지와 다른 자원을 가지기 위해 가상 주소 공간을 포함한다.

- Scheduling / Execution
  > 프로세스의 실행은 다른 프로세스들과 상호작용한다. 어떻게 상호작용할지에 대한 방식은 스케줄링 기법에 따라!

- dispatching의 단위는 thread 혹은 lightweight process라 불린다.
- resource ownership의 단위는 process 혹은 task로 불린다.
- Multithreading
  > 단일 프로세스 내에서 다중, 동시적 경로로 실행할 수 있게 OS가 지원해주는 기능
<br/>

## 2. Threads
- 각 쓰레드는 실행 상태를 지닌다.(ex. Running, Ready, etc)
- 실행중이 아니라면 thread context를 저장한다.
- 실행 스택을 가진다.
- 프로세스내 메모리, 자원에 접근가능하며 같은 프로세스내 존재하는 다른 쓰레드들과 공유한다.
- Multithread process model에서는 single thread process model과 달리 pcb만 존재하는게 아니라 더 작은 크기의 tcb(thread control block)의 정보만 바꿔서 스케줄링한다.

- 프로세스에 존재하는 모든 쓰레드에 영향을 주는 행위
  * **Suspending** : 프로세스가 suspend되면 모든 쓰레드 또한 suspend 된다. 왜냐하면 프로세스내 쓰레드들은 모두 같은 공간 주소를 공유하고 있기 때문이다.
  * **Termination** : 프로세스가 메인 메모리에서 삭제되면 프로세스내 모든 쓰레드 또한 삭제된다.

- Thread execution states
  * key states : Running, Ready, Blocked
  * others : Spawn, Block, Unblock, Finish 

<br/>

## 3. Key benefits of Threads
쓰레드의 장점 4가지를 살펴보자.
- 프로세스를 새로 생성하는것 보다 쓰레드를 만드는데 걸리는 시간이 더 적다.
- 프로세스를 종료하는데 걸리는 시간보다 쓰레드를 종료하는데 걸리는 시간이 더 짧다.
- 프로세스간의 교환보다 쓰레드간의 교환이 시간적으로도 적고 오버헤드도 적다.
- 쓰레드는 실행중인 다른 프로그램간의 효율적인 소통을 강화시켜준다.

<br/>

## 4. Types of Threads
#### 1. ULT(User Level Thread)

- 모든 쓰레드 관리는 어플리케이션에 의해 된다.
- 커널은 쓰레드의 존재를 알지 못한다. (왜냐하면 user space와는 독립적이기 때문)
- 프로세스 스케줄링과 쓰레드 스케줄링은 독립적.
- ULT의 장점
	* Thread switching시 커널 모드의 권한을 요구하지 않는다.
	* 스케줄링이 어플리케이션에 특화될 수 있다.
	* ULT는 유저 레벨에 존재하기 때문에 어떠한 OS에서도 작동가능하다.

- ULT의 단점
	* 만약 System call을 실행하면 해당 쓰레드만 블록되는게 아닌 프로세스내 모든 쓰레드가 블록된다.
	* multiprocessing 환경에서 장점을 활용하지 못함. 왜냐하면 커널은 한 프로세스당 한 프로세서만 할당하기 때문에 단일 쓰레드만 실행이 가능하다.

#### 2. KLT(Kernel Level Thread)

- 쓰레드 관리는 커널로 부터 된다.
- KLT의 장점
	* 프로세스내 한 쓰레드가 블록되어도 커널은 다른 쓰레드로 스케줄링할 수 있다.
	* 멀티 프로세서에서 같은 프로세서에게 멀티 쓰레드를 일제히 스케줄링할 수 있다.
- KLT의 단점
	* 한 쓰레드에서 다른 쓰레드로 전환하기 위해서 항상 mode switching이 필요하다. fork, signal wait의 연산량을 비교할때 ULT보다 KLT가 확연히 높다.

**정리하자면 ULT의 장점, 단점이 결국 KLT의 단점, 장점이 된다. 상반되는 특징을 지니고 있다.**
