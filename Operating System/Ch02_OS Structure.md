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
  
