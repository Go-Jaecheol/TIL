
### 💾 Process??
(Program vs. Process)

프로세스는 프로그램이랑 다르게 현재 실행 상태를 나타낸다.  
이런 점에서 프로그램은 passive 하고, 프로세스는 active 하다고 볼 수 있다.  
또한, 하나의 프로그램에서 하나 이상의 프로세스가 실행될 수 있다.

> ***❗️ 그러면 Process가 어떤 상태를 나타내는데 ??***  
* **Current Instruction (Program Counter)**  
  - PC 레지스터를 통해 현재 실행중인 instruction의 주소 값을 나타낼 수 있음  
* **레지스터 상태**  
* **메모리 정보 (stack, heap, global variables)**  
* **Parent 정보**  

---

### 📄 Process Memory Space Layout  
(가상 메모리 공간)

![](https://images.velog.io/images/jfe/post/79d7188d-d030-4255-8917-010d6072fcc7/memory%20layout.png)

가상 메모리 공간으로, 프로그래머는 이 가상 메모리 공간을 바탕으로 프로그램을 짜면 된다.  
물론, 실제 메모리 공간이랑은 다르다.  
32비트 CPU의 경우, 각 프로세스는 4GB의 메모리 공간이 있다고 가정하고 돌아가는데  
이 때, 각 프로세스는 실제 메모리 공간에서 훨씬 작은 메모리만을 사용한다.  
하지만 이런 변환은 OS가 다 해주기 때문에 프로그래머는 가상 메모리 공간만 신경쓰면 된다.  

* #### Text 영역  
  - 모든 프로그램(Binary program code)은 Text 영역에 존재한다.  
  - data가 아닌 것들 존재  
* #### Data 영역  
  - global variable(전역 변수)가 존재  
* #### Heap 영역  
  - 동적 할당을 요청 받으면 [ex. malloc()] heap 영역에 존재하는 메모리 공간을 반환해주고 grow up 한다.  
  - heap 영역은 메모리 공간에서 위로 자라게 된다. (grow upward)  
* #### Stack 영역  
  - 함수가 호출되면 stack에 함수 필요값들(지역변수, 파라미터, return주소)을 저장한다.  
  - heap 영역과는 반대로 아래로 자란다. (grow downward)  
  
* #### Kernel을 위한 공간  
  - 32비트 CPU의 경우, 전체 4GB의 공간에서 1GB가 커널을 위해 사용된다.  
  - Kernel Stack은 system call에 의해 사용된다.  
  (text 영역에서 system call이 호출되면 kernel 영역으로 jump한 후 실행되고 다시 반환된다.)  
  (이 때, kernel stack 영역이 temporary area로서 사용된다.)  
  
  <img src="https://images.velog.io/images/jfe/post/ec4c9bb0-8cd1-40e3-a282-ecc980a67846/memory%20layout%202.png" width="700">
  
  - 가상 메모리 공간에서 메모리가 사용될 때, OS가 실제 Physical 메모리에 할당해준다.  
  - 실제 메모리 공간에 할당할 때는 OS가 알아서 저장 위치를 설정해준다.  
  - 단, kernel memory area는 프로세스마다 공유되는데 그래서 kernel stack은 프로세스 하나당 하나씩 공간을 차지하게 된다.  
  
---

### 📌 Process State

<img src="https://images.velog.io/images/jfe/post/523923b4-bdd7-433d-bffe-ce5069164c1b/process%20state.png" width="700">

(state는 정의하기에 따라 다름)  
* #### New  
  - 프로세스가 처음 만들어진 상태  
* #### Ready  
  - Running 할 준비가 된 상태  
  - OS가 스케쥴링을 통해 하나의 적합한 프로세스를 running 상태로 넘김  
  - **ready queue**  
    - 프로세스는 항상 하나만 돌아감 (CPU 코어당 하나)  
    - 한 프로세스가 돌아가는 동안 나머지 프로세스들은 ready queue에서 대기  
* #### Running  
  - Instruction이 실행되는 상태  
  - CPU 코어당 하나의 프로세스만이 Running 상태가 될 수 있음  
  - 한 프로세스가 너무 오래 사용되면 preempted 되어 ready 상태로 돌아가게 됨  
* #### Terminated  
  - 프로세스가 종료된 상태  
* #### Waiting  
  - I/O나 다른 디바이스와 상호 작용해야 할 때 waiting 상태로 넘어가게 된다.  
  (디스크를 읽거나 디바이스와 상호 작용하는 경우에는 시간이 걸리므로 CPU가 쉬게 된다.)  
  (CPU가 쉬는 경우가 생기면 안되기 때문에 프로세스를 waiting 상태로 보내야 함)  
  - 여러 프로세스가 waiting 상태에 넘어와 기다릴 수 있기 때문에 **waiting queue** 존재  
  - waiting 상태가 끝나면 다시 ready 상태로 넘어감 (ready queue에 추가)  

---

### 📦 PCB (Process Control Block)

: 프로세스를 표현하는 커널 안에 있는 데이터 구조체  

PCB는 간단하게 설명하면 프로세스가 동작하는데 필요한 정보들을 모아놓은 구조체다.  
프로세스가 계속 Running 상태에 있지 않고 결국에는 쫓겨나게 되는데 이후에 다시 Running 상태로 돌아올 때를 대비해서 지금까지의 정보들을 저장해둔다.  
![](https://images.velog.io/images/jfe/post/57ee2f70-405e-4266-bff3-066cfde9d7fa/pcb.png)

> ***❗️ 그러면 PCB에 저장하는 정보는 ??***  
* **Process State**  
  - New, Ready, Running, Waiting 등...   
* **Program Counter**  
* **CPU registers**  
  - 레지스터에 저장된 값들을 다 copy  
* **CPU scheduling 정보**  
  - 스케쥴링에 필요한 정보들을 모두 저장  
  - 프로세스 우선순위, scheduling queue 포인터, scheduling 파라미터  
* **메모리 관리 정보**  
  - 메모리 관리를 위한 추가적인 구조체 정보를 모아놓음  
* **Accounting 정보**  
  - CPU 사용시간, PID 등 리소스 사용량 추적을 위한 정보들을 저장  
* **I/O status 정보**  
  - I/O device 사용 관련 정보들  
  - I/O device 리스트, open 된 파일들 리스트  

***ex) Linux에서 PCB***  

![](https://images.velog.io/images/jfe/post/c2269ba9-f270-4229-a19c-87f28aa716a9/pcb%20linux.png)

PCB 안에 포인터를 통해, PCB끼리 **doubly linked-list**로 연결되어 있다.  
current라는 변수를 통해 현재 실행중인 프로세스를 가리킴  

---

### 🕹 Process Switch  

![](https://images.velog.io/images/jfe/post/c1929996-e964-4ee3-9b80-9eef08d8de46/process%20switch.png)

1. system call이나 interrupt가 일어나면 kernel mode로 들어감  
(이 과정에서 kernel mode로 넘어간 P0 프로세스는 잠시동안 OS가 됨)  
2. kernel mode에서 PCB0의 state를 저장하고 PCB1의 state를 불러와서 P1 프로세스로 **Switch**  
3. P1 프로세스에서 다시 다른 프로세스로 넘어갈 때도 같은 과정을 반복  

* #### Context Switch  
  - 현재 프로세스 상태를 저장하고 다른 프로세스 상태를 불러오는 작업  
  - 프로세스가 **실제로 작업하는 시간**과 **context switch time**을 합친 구간에서 **context switch time**이 차지하는 비율이 낮을수록 효율적  
  - H/W를 통해 context switch time을 줄일 수 있음  
    - multiple register set
    : 레지스터 set을 더 만들어 메모리에 갔다오는 시간을 없애자  
    - simply change the pointer  

---

### 📆 Process Scheduling  

프로세스를 sequential하게 작동시키면 waste time이 발생한다.  
=> 이를 해결하기 위해 Process Scheduling 사용  
(프로세스를 일정 시간 간격으로 아주 빠르게, 짧은 시간 내에 여러 번 switching 해서 프로세스들이 병렬적으로 실행되는 것처럼 보이게 함)  

* #### Scheduling Queues  

  <img src="https://images.velog.io/images/jfe/post/36413ffc-d50e-4c41-953d-40897fb0998e/queue%20diagram.png" width="700">
  
  - Job queue  
    - 프로그램이 시작될 때, 사용할 Job들을 등록만 해놓은 상태  
    - Job queue -> Ready Queue  
    - 초창기 컴퓨터에 해당, 요즘 컴퓨터에는 쓰이지 않음  
  - Ready queue  
    - 바로 running 상태로 넘어갈 준비가 되어있는 프로세스들이 모여있음  
    - linked list로 연결되어 있음  
    - 일반적인 스케쥴링 대상  
  - Device queue (Wait queue, I/O queue)  
    - 디바이스를 사용하기 위해 대기하는 큐  
    - 디바이스를 사용해야 하는 경우, PCB를 ready queue에서 떼서 wait queue에 연결한다.  
    - 디바이스 사용이 끝나면 그 PCB는 다시 ready queue에 연결된다.  

* #### Scheduler  
  - Long-term scheduler  
    - job scheduler  
    - 컴퓨터가 다른 job을 돌릴 여유가 되면 job queue에서 프로세스를 고름  
    - 천천히 돌아감, 결정 시간 단위가 길다.  
  - Short-term scheduler  
    - CPU scheduler  
    - ready queue에서 다음 돌아야 할 프로세스를 고름  
    - 아주 빨리 돌아가며, 순간적으로 결정  

* #### Scheduling 기준  
  - CPU 사용량 최대화 하는 방법 (I/O 잘 안쓰는 프로세스 고르기)  
  - 평균 waiting time 최소화 하는 방법  
  - response time 최소화 하는 방법  
  - 순차적으로 고르기 (-> 모든 프로세스가 동일한 CPU 시간 이용 가능) 
  
---

### ⌨️ Operations on Processes  

![](https://images.velog.io/images/jfe/post/9c1cca89-476c-442a-9f73-1edd29ed2fe0/process%20tree.png)

* #### 프로세스 생성  
  - 하나의 프로세스는 여러 개의 child 프로세스를 만들 수 있다. (트리 구조)  
  - ***init*** process (pid = 1)라는 모든 프로세스의 부모가 되는 프로세스가 있다.  

* #### 리소스 할당 방법  
  - child 프로세스에게 리소스를 필요한만큼 주는 방법  
  - child 프로세스에게 parent 프로세스의 리소스를 할당 받는 방법  

* #### child 프로세스 동작 방식  
  - parent와 child가 동시에 run 하는 방식  
  (parent와 child가 같이 ready Q에 있음, 동등하게 경쟁)  
  - child가 종료될 때까지 parent가 기다리는 방식  
    - child를 통해 일을 해서 결과를 받기 위한 목적이 많음  
    - child를 생성한 후, parent는 ready Q에서 나와서 waiting 상태가 됨  
* #### child 프로세스 프로그램 종류  
  - parent가 그대로 복사된 형태  
  - parent와는 별개의 새로운 형태  

---

### 🖥 Linux에서 프로세스 생성  

* #### fork() system call  
: 새로운 프로세스를 생성함  
  - parent와 child가 동시에 run  
  - parent가 그대로 복사된 형태  
  - return 값은 다름 (parent: child의 PID, child: 0)  
* #### exec() system call  
  - 호출하는 순간 프로세스 메모리의 내용을 완전히 새로운 것으로 바꿈  
  (바꿀 대상 프로그램 이름을 지정해서 사용)  
  - return 값을 받을 수 없다 !  
  (exec 하는 순간 프로세스 내부가 날아가고 새로운 프로그램으로 채워짐, exec를 호출 한 사실도 날아가버림)  

* #### wait() system call  
: parent가 child가 종료될 때까지 기다림  
> ***❗️ Parent가 wait() 안하면 ??***  
child가 종료되면 종료한 status 정보가 생긴다.  
status 값을 OS가 parent에게 전해줘야 하는데 wait()를 하지 않으면 줄 수가 없다.  
그러면 OS가 이 값을 계속 들고 있어야 되는데 이 상태를 **좀비 프로세스**라고 한다.

  > *** ❗️❗️ Parent가 wait()을 계속 호출하지 않으면 !?***  
  init 프로세스가 주기적으로 parent가 돼서 wait가 안되고 쌓여있는 child를 wait() 시킨다. 

---

### ❌ 프로세스 종료  

프로세스를 종료시키기 위해 exit() 함수가 호출된다.  
그러고 나서 status 값이 parent에게 반환되고, 리소스들이 다시 OS 관리로 넘어간다.  
exit()라는 코드를 명시적으로 사용할 수도 있고, 명시하지 않고 main 함수가 끝나면 자동으로 exit()가 불리도록 하는 방식도 있다.  

> ***❗️ Parent가 child를 강제로 종료시키는 경우도 있을까 ??***  
* child가 리소스를 너무 많이 사용하는 경우  
  - 메모리가 부족하면 crash가 일어날 수도 있으니 child를 강제 종료시킨다.  
* child가 더 이상 필요하지 않는 경우  
* parent가 종료된 경우  
  - parent가 종료되면 그 child를 DFS를 통해 찾아가면서 다 종료시킨다.  
  
