### 🔗 운영체제 형태  

<img src="https://media.vlpt.us/images/jfe/post/b027324c-85df-446e-ba18-446a067fb62f/OS%20service.png" width="700">

* **user interfaces**  
: user와 OS가 서로 상호 작용할 수 있도록 제공  
ex) GUI, batch, command line 등  
* **system calls**  
: Application이 OS와 상호 작용하기 위한 유일한 방법  
* **services**  
: Applications request services to OS  

---

### 🙋 사용자 관련 OS 서비스  

* #### User Interface  
    - **CLI**
    : Command-Line Interface 
    ex) 터미널  
    - **GUI**
    : Graphical User Interface  
    ex) Windows, Mac, 스마트폰 등  
    - **batch**  
    : 여러 프로그램을 미리 순서 정해놓고 일괄적으로 처리  
* #### 프로그램 실행  
* #### I/O operations  
* #### File system 처리  
    - OS 없이는 파일 시스템에 접근할 수 없음  
* #### 네트워크 통신  
    - 프로세스간의 데이터 communication 포함  
* #### Error Detection  
    - 에러가 발생하면 이를 감지해서 조치를 취함 
    - 대부분의 OS는 에러가 발생하면 프로그램 종료  
    - ex) Bit-flip, memory error, power failure 등  

---

### 🗝 효율성 관련 OS 서비스  

* #### Resource 할당  
    - CPU cycles, 메모리 공간, 메모리 bandwidth 등  
* #### Accounting  
    - 각각의 프로세스들이 리소스(CPU, 디스크 메모리 등)를 어느 정도 썼는지 추적  
* #### Protection & Security  
    - 하나의 프로세스가 리소스를 모두 사용하는 것을 방지 
    - 프로세스 간 간섭을 막아줌  
    > _" A system is as secure as its weakest link "_  


---

### 🎨 OS Design

* #### Design 목표  
  - **사용자 관점**  
    - 사용하기 쉽도록  
    - 배우기 쉽도록  
    - fast  
    - 쉽게 죽지 않도록 (reliable) 
  - **시스템 관점**  
    - 구현하기 쉽도록  
    - 유지 보수하기 쉽도록 (version 업데이트 하기 편하게)  
    - 운영하기 쉽도록 (여러 대의 컴퓨터 운영하기 편하게)  
    - flexible & efficeint  
    - 고장에 대한 대비가 잘 되어있는지 (fault-tolerant)  
* #### 가이드라인 : Mechanism & Policy  
  - **Mechanism**  
  : 기술적인 측면, 어떤 Policy에 대해 어떻게 실행할건지  
  - **Policy**  
  : 어떤 것을 하고 싶은 지에 대한 정책, 기술적인 것 x  
  > Policy가 바뀔 것에 대비해서 Mechanism을 개발할 때는 파라미터화 하는 것이 중요!  
  (*Separation of policy from mechanisms*)  
  
---

### 🧩 OS Implementation

* #### Assembly language  
  - **장점**  
    - difficult & tedious  
    - 에러가 생기기 쉬움  
    - 다른 환경의 컴퓨터에 돌리기 어려움 (difficult to port)  
  
* #### High-level language  
  - **장점**  
    - 구현 시간이 빠름  
    - 코드가 간략하고 이해하기 쉬움  
    - 컴파일러가 발전하는만큼 한번 생성된 코드의 성능도 좋아짐  

> 우선 high-level language로 코드를 짜고, 그 다음에 assembly language로 바꾸는 방법이 좋음  

---

### 🗿 OS 구조  

* #### Simple Sturcture  
  <img src="https://images.velog.io/images/jfe/post/b3faf602-2a4c-4984-b8bf-9927c18a60a5/simple%20structure.png" width="700">  
  
  - 간단하게 시작되었지만 기능이 계속 추가되면서 복잡해지고, 시스템적으로 중앙 통제에서 벗어남  
  - 크게 보면 layer로 나눠져 있어서 Monolithic(하나의 거대한 형태) 하지 않지만, 커널 부분은 tightly-coupled 되어있음  
  
* #### 이상적인 구조 (Layered Approach)  
  <img src="https://images.velog.io/images/jfe/post/b6e6af11-574e-449f-ab29-04fc2972496a/layered%20approach.png" width="400">  
  
  특정 layer에서는 자신의 바로 밑에 있는 레이어의 서비스를 이용한다. (함수 호출)  
  
  - **장점**  
  : 간단하고, 디버깅 하기 쉬움  
  (아래 레이어의 구현을 바탕으로 구현했기 때문에, 해당 레이어에서 새로 생긴 버그만 해결하면 됨)  
  
  - **단점**  
  : 레이어를 나누기가 힘들고, 레이어끼리 함수 호출하는 과정에서 overhead(과부화)가 발생할 수 있음  
  
---

### 📍 Microkernel

<img src="https://images.velog.io/images/jfe/post/9e31a403-72c9-403d-b86d-e65a154aab3f/microkernel.png" width="700">  

: 핵심 기능만 kernel에 남기고, 나머지는 빼버린 최소화된 커널  
 (**Monolithic 반대**)

* #### 장점  
  - 다른 하드웨어에 port 하기 쉬움 (커널이 작기 때문에)  
  - 더 안전하고 reliable (커널이 작아서 공격 받을 부분도 적기 때문에)  
* #### 단점  
  - 성능이 상대적으로 안좋음  
  (Monolithic에서는 request&response가 한 번씩만 일어나면 되는데, Microkernel에서는 여러번 일어나기 때문에)  
  
---


### 📞 System Calls

Program이 직접 작업을 할 수 없기 때문에, system call (Interrupt Mechanism)을 통해 OS에게 요청(request)  

* #### System Call의 3가지 기능  
  1. User mode에 있는 응용 프로그램이 커널의 기능을 사용할 수 있게 해줌  
  2. System Call을 통해 User mode에서 Kernel mode로 전환  
  3. 커널에서 시스템 호출이 끝나면, Kernel mode에서 User mode로 돌아가서 작업 계속 진행  
  
---

### 🔗 System Call & API 관계

System Call을 직접 호출할 수 있지만, 실제로는 보통 library function(API)를 통해 System Call을 간접적으로 사용함  
(대표적으로 libc(리눅스 계열 라이브러리)가 있는데 libc를 통해 system call을 호출, 이때 하나의 libc가 system call을 여러 개 호출할 수도 있고 반대가 될 수도 있다.)  

> ***그러면 왜 API를 더 많이 사용할까 ???***

  - System call은 너무 하위 레벨이기 때문에, 부가적인 작업이 많이 필요해서 까다롭다.  
  - Portability를 위해! (다른 환경에서도 쉽게 컴파일 될 수 있도록)  
  (API는 함수의 이름이나 파라미터와 같은 프로토 타입이 불변하기 때문에 여러 환경에서 사용 가능)  

---

### ⚙️ System Call Interface

<img src="https://images.velog.io/images/jfe/post/1d629af1-1a53-4b1d-9bcd-d72add738f22/System%20call%20interface.png" width="700">

1. User application이 API 함수를 통해 system call을 호출  
2. User mode에서 Kernel mode로 **mode switch**  
3. 해당 함수에 맞는 system call을 system call function pointers에서 찾은 후  
4. application이 요구한 값을 return  
> *이 과정에서 사용자는 system call이 어떻게 구현되어 있는지는 알 필요 x*  
(Interface와 이게 어떤 값을 return 하는지만 알면 됨)  

---

### 📂 System Call 종류

* Process control  
* File manipulation  
* Device manipulation  
* Information maintenance  
* Communications  
* Protection  

---

### ✉️ System Call : Communications

* #### Message passing
  - 메세지를 직접 주고 받음, 소켓 프로그래밍(send, receive)  
  - 대상이 누군지 분명히 알고 있어야 함  
  - connection을 먼저 만들어진 상태여야 함  
  
* #### Shared-memory model  
  - 공유 공간을 이용해서 메세지를 공유 (메모리에 적는 방법이 훨씬 성능이 좋음)  
  - 원래 서로 다른 프로세스는 메모리가 철저히 분리되어 있어서 상대방 메모리를 볼 수 없지만  
  - Shared-memory는 예외 케이스 (특정 일부 공간을 프로세스끼리 공유)  
