## 1.OS에서 Kernel이 하는 역할은 무엇이고 어떠한 종류가 있는가?

### 1-1.OS에서 Kernel이 하는 역할

### Overview - [kernel(Operating system) - wiki](https://en.wikipedia.org/wiki/Kernel_(operating_system))

- kernel은 컴퓨터 운영체제에서 핵심 역할을 담당하며, 시스템의 모든 것을 완벽히 제어합니다.
- 이는 컴퓨터 메모리에 항상 존재하는 운영체제 코드 중 일부이며, 하드웨어와 소프트웨어의 Interaction을 용이하게 하는 역할을 합니다.
- 운영체제에서 시스템의 대부분을 제어하는 프로세스인 만큼, 가장 첫 번째로 로드 되어지는 프로그램입니다. 커널이 가장 먼저 로드 된 후, 나머지 셋업 프로그램을 메모리에 올릴 뿐만 아니라 메모리, 주변기기, I/O 요청 등을 CPU를 위한 data processing 명령어로 변환을 해줍니다.
- kernel의 주요 코드는 메모리의 분리된 공간에 로드 되어지고 어플리케이션단과 다른 중요하지 않은 os의 부분으로부터 접근이 보호되어 집니다. **우리는 이러한 공간을 kernel space 라고 부르며 이 곳에서는 process 실행, hardware 장치 관리, 인터럽트 처리 등의 작업이 수행됩니다.** **이와 다르게 브라우저나, 워드 프로세서, audio or video 플레이어는 user space 라고 불리는 또 다른 분리된 공간을 사용합니다.**
- 이러한 분리는 사용자 데이터와 커널 데이터가 서로 방해받는 것을 막을 수 있고 분안정성과 지연을 막을 수 있습니다. 뿐만 아니라 어플리케이션이 다른 어플리케이션으로부터 오작동 하는 것을 막아 줄 뿐만 아니라 운영 체제 전체에 장애가 전파되는 것을 막아줍니다.
- 커널의 인터페이스는 low-level abstraction layer 입니다. 프로세스가 커널로부터 서비스를 요청할 때, 일반적으로 wrapper 함수와 같은 시스템 콜을 통해서 이를 호출해야 합니다.

> ### System call
>
> - 컴퓨팅에서 system call은 컴퓨터 프로그램이 서비스가 실행되어질 운영체제의 커널에게 서비스를 요청하는 것을 의미합니다. 
> - 이러한 system call에는 하드웨어 관련된 서비스, 새로운 프로세스의 생성과 실행, process scheduling과 같은 내부 커널 서비스와의 통신 등이 포함하구요, 이는 process와 operating system 간의 본질적인 인터페이스라고 생각하면 됩니다.



### Random-access memory

kernel은 각 프로세스가 어떤 메모리를 사용해야하는지 결정할 뿐만 아니라 가용 메모리가 충분치 않을 경우에 어떠한 조치를 취해야 하는지에 대해서도 결정할 책임을 집니다.



### Input/output devices

kernel은 적절한 기기가 I/O 를 수행하기 위해 어플리케이션의 요청을 각 기기에 할당해줄 뿐만 아니라 device 사용을 위헌 편리한 방법을 제공해줍니다.



### Resource management

resource management의 주요 포인트는 도메인 내의 자원의 접근을 조정(mediate)하기위해 사용되어지는 보호 메커니즘과 실행 도메인(address space)를 정의하는 것 입니다. 커널은 동기화와 IPC를 위해 메소드를 제공해줍니다. 여기서 말하는 메소드는 시스템 콜에 가깝습니다. 

커널은 process와 threads 사이에 context switching의 책임도 있습니다.

> ### Context Switching
>
> Context Switching 이란 process 또는 thread의 상태를 저장해서 수행된 최종 지점에서 실행을 재게하려는 목적을 가지고 있습니다. 이는 여러 프로세스가 단일 CPU를 공유하게끔 만들어 주고 이는 multitasking os의 핵심기능 입니다.

### Memory management

kernel은 시스템 메모리에 대한 전체 엑세스 권한을 가지며 프로세스가 필요할 때 이 메모리에 안전하게 액세스할 수 있도록 허용해야합니다. 종종 이 첫 번째 단계는 가상 메모리를 통해 수행되어지는데, 이는 paging 또는 segmentation을 통해 수행되어집니다.

가상 메모리는 커널이 주어진 물리적 주소를 다른 주소인 가상 주소로 표시할 수 있도록 합니다. 가상 주소는 프로세스마다 다를 수 있으며, 한 프로세스가 특정 가장 주소에 접근하는 메모리는 다른 프로세스가 동일한 주소에 접근하는 메모리와 다를 수 있습니다. 이는 개별 프로세스가 os 에서 오직 하나만 실행중인 것 처럼 행동하게 만들어주고 서로간에 충돌이 발생하는 것을 막아줍니다.

**많은 시스템에서 프로그램의 가장 주소는 현재 메모리에 없는 데이터를 참조할 수 있습니다. 가상 주소에 의해 제공되어지는 간접 계층은 os가 하드 디스크와 같은 데이터 저장소를 사용하여 메인 메모리에 남아 있어야하는 내용을 저장할 수 있습니다.** **결론적으로 운영체제는 시스템이 물리적으로 이용 가능한 메모리 이상을 프로그램이 사용가능하도록 만들 수 있습니다.**

프로그램이 시스템 메모리에 현재 존재하지 않는 데이터가 필요하다면, CPU는 커널에게 신호를 보내고 커널은 필요하다면 비활성화된 메모리 block을 디스크에 기록하고 프로그램에 의해서 요청되었던 데이터를 교체합니다. 결국 프로그램은 멈춘 지점부터 다시 실행을 재게할 수 있게 되어집니다. 이러한 과정이 바로 잘 알려진 demand paging 입니다.

가상 주소는 메모리에서 커널을 위해 보호되는 공간(kernel space)과 어플리케이션을 위해 보호되는 공간(user space)의 생성을 가능하게 끔 만들어 줍니다. 어플리케이션은 processor에서 커널 메모리 주소를 지정하는 것을 허용하지 않음으로 어플리케이션이 동작하는 커널로부터 손상받는것을 막아줍니다.

이러한 기본적인 메모리 공간 분리는 실제 범용 커널의 디자인에 많은 기여를 했고 이는 여러 시스템에서 통용적으로 사용되어 왔습니다.

> ### Virtual addressing
>
> 프로그램에 가상 주소를 부여해서 실제 자주 사용 되어지거나 되어질 부분만 메인 메모리에 위치시키고 나머지 부분은 하드 디스크와 같은 저장장치에 위치시키는 방법이다. 
>
> 가상 주소는 메인 메모리의 실제 물리 주소에 맵핑되어지는데, 프로그램이 가리키는 가장 주소가 메인 메모리에 없을 경우 커널은 디스크로부터 이를 검색해서 해당 데이터를 메모리에 위치시키게 끔 하는 방법이다.
>
> 이를 통해 가용 메모리 이상의 프로그램을 OS가 실행할 수 있으며, 각 프로그램 별로 메모리 접근이 격리된다.
>
> ### Memory Paging
>
> 운영체제에서 memory paging은 메인 메모리 사용을 위해 2차 저장장치(이하 디스크)에 데이터를 저장하거나 검색하기 위한 메모리 관리 기법을 말한다. 이 방법에서 OS는 동일한 사이즈의 block을 의미하는 page 단위로 디스크로부터 데이터를 검색한다. Paging은 현대 OS의 가상 메모리 구현의 중요한 부분 중 하나로 디스크를 사용함으로써 Program이 물리 메모리 크기를 초과할 수 있도록 한다.
>
> **즉, 메모리를 Page라고 불리는 동일한 사이즈의 block으로 나누어서 관리하는 방법이고, Virtual Memory 구현에서 주로 사용되는 핵심 개념이다.**
>
> ### Memory Segmentation
>
> Memory segmentation은 컴퓨터의 주요 메모리를 segments 또는 sections로 나누는 OS 메모리 관리 기술을 의미한다. Segmentation을 사용하는 컴퓨터 시스템에서, 메모리 위치를 참조하는 것은 세그먼트와 세그먼트 내의 offset(memory location)을 식별하는 값을 포함한다. 
>
> Segments는 보통 개인 routines 또는 data 테이블과 같은 개념 단위의 분리와 일치하기 때문에 paging보다는 프로그래머에게 더 친숙하게 보여지곤 한다. 또 다른 Segments들은 프로그램의 서로 다른 모듈, code 또는 data segments와 같은 메모리 사용의 분류로 생성되어지곤 한다. 
>
> Segmentation은 원래 system software가 그들이 사용하는 process 또는 데이터로부터 분리되어지는 방법으로써 발명되었다. 또 이는 동시에 여러 프로세스가 수행되는 시스템의 안정성을 증가시키기 위해 의도 되어졌다. 
>
> 하지만 x86-64 아키텍처에서 이는 legacy로 구분되었고 대부분의 x86-64 기반의 현대 시스템 소프트웨어는 더이상 memory segmentation을 사용하지 않는다. 대신에 메모리 보호를 위한 방법으로써 제공되어진 memory-paging 방식을 사용해서 그들의 프로그램과 데이터를 다룬다.
>
> 하지만 대부분의 x86-64 구현은 Memory Segmentation을 이전 버전과의 호환성을 위해서 여전히 지원합니다.
>
> **Memory Segmentation이란 메모리를 정해진 사이즈의 block이 아닌 각각 다른 개념 단위의 사이즈를 가진 block으로 관리하는 방법이고, External Fragmentation을 막을 수 없기 때문에, 현재 적절하지 않은 레거시로 간주되어서 사용되어지지는 않는다.**

### Device Management

유용한 기능을 실행하기 위해선 process들은 컴퓨터에 연결된 주변기기에 접근할 필요가 있습니다. 이는 device drivers를 통해서 커널에 의해 관리되어집니다. device driver는 OS가 하드웨어 device와 상호작용을 가능하게 하는 컴퓨터 프로그램입니다. 이는 운영체제가 특정 하드웨어를 제어하고 통신하는 방법에 대한 정보를 제공합니다. 

**driver는 응용 프로그램에서 중요한 부분이며, driver의 설계 목표는 추상화 입니다. driver의 기능은 OS가 지정한(mandated) 추상 함수 호출을 device 특정 호출로 변환해주는 것 입니다.** 이러한 이론에서 device는 적절한 driver와 함께 동작한다면 올바르게 수행되어질 수 있습니다. Device driver는 video cards, sound cards, printers, scannders, modems, LAN cards 등과 같은 것들을 위해서 수행되어집니다.

hardware 계층에서 device driver의 공통 추상화는 다음을 포함합니다.

- 직접적인 Interfacing
- 상위 계층의 인터페이스 사용
- 하위 계층의 device driver 사용 (file drivers using disk drivers)
- 완전히 다른 작업을 수행하면서 하드웨어로 작업 시뮬레이션

software level에서, device driver 추상화는 다음을 포함합니다.

- os가 하드웨어 자원에 직접적으로 접근하게끔 허용
- Only implementing primitives
- TWAIN과 같은 non-driver software를 위한 인터페이스구현
- 언어 구현

예로들어 사용자에게 화면에서 무언가를 보여주기 위해, 어플리케이션은 커널에 요청을 보내고, 커널은 display driver에 요청을 전달하고 display driver는 실제로 문자나 픽셀등을 plotting할 책임이 있습니다.

kernel은 이용가능한 장치의 리스트를 유지해야합니다. 이 리스트는 미리 알려져야하고, 사용자에 의해서 설정되어져야하고 런타임시에 os에 의해서 발견되어져야 합니다. **plug and play 시스템에서, device manager는 Peripheral Component Interconnect(PCI) 또는 Universal Serial Bus(USB)와 같은 다른 주변(peripheral) 버스에서 scan을 수행하고 설치된 장치들을 검색한 후 적절한 driver를 검색합니다.** 

장치 관리는 OS에 매우 특화된 주제이기 때문에, 이러한 드라이버들은 kernel 설계 종류에 따라 각각 다르게 다루어집니다. 하지만 모든 경우 커널은 포트나 메모리를 통해 driver가 장치에 물리적으로 접근할 수 있게 I/O를 제공해주어야 합니다. 

장치 관리 시스템을 디자인 할 때 중요한 결정을 내려야합니다. 일부 설계에서 장치에 접근하는 것은 Context Switches를 포함할 수 있기 때문에 이는 CPU를 매우 많이 사용하고 쉽게 상당한 성능 오버헤드를 유발할 수 있습니다.

### 1-2. 커널에는 어떠한 종류가 있을까요?

#### 1-2-1. Monolithic Kernel

모든 OS 서비스는 monolithic kernel 구조에서 main kernel thread와 함께 실행되므로 동일한 메모리 영역에 존재합니다. 이러한 접근 방법은 풍부하고 강력한 hardware access를 제공합니다. 

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Kernel-simple.svg/170px-Kernel-simple.svg.png)

monolithic kernels의 주요 단점은 시스템 구성요소들 간의 의존성입니다. 이게 왜 문제가 되냐면 하나의 커널이 모든 작업을 처리하게 되는데, 만약 하나의 작업에서 버그가 발생한다면 이는 전체 시스템에 충돌을 야기시킬 수 있기 때문입니다. 

**Unix와 같은 운영체제에서 사용되어진 Monolitihc kernels는 OS의 모든 기능과 장치 driver들을 포함합니다. monolitihic kernel은 모든 커널과 관련된 작업을 수행하기 위해 필요한 모든 코드를 포함시키는 하나의 단일 프로그램을 의미하기도 합니다.**

monolithic 커널의 장점은 다음과 같습니다. 

- software가 다른 커널 구조에 비해서 작기 때문에 더 빠릅니다.
- 하나의 단일 소프트웨어이므로 소스 및 컴파일된 산출물의 크기가 더 작습니다.
- 코드의 크기가 작다는 것은 일반적으로 보안 문제를 줄일 수 있다는 것을 의미합니다.

monolithic 커널 에서의 대부분의 작업은 system calls를 통해 이루어집니다. 기본적으로 프로그램 내에서 수행되어지는 호출과 호출의 복사본은 system call을 통해서 전달되어집니다. 

**monolithic Linux kernel은 동적으로 모듈을 로드하려는 목적 뿐만 아니라 customizing의 용이성 때문에 매우 작게 만들어졌습니다. 커널을 소형화(miniaturize) 시키는 능력은 임베디드 시스템에서 Linux 사용의 급속한 성장으로 이어졌습니다. 이러한 유형의 커널은 os의 핵심 기능과 런타임에 모듈을 로드할 수 있는 장치 driver로 구성됩니다.** 

monolitihc 커널의 단점은 다음과 같습니다.

- 커널 한 부분에 발생한 버그들은 강력한 side effects를 가질 수 있다. 커널의 모든 기능에는 모든 권한이 있기 때문에 한 기능의 버그는 다른 기능의 데이터 구조, 커널의 관련없는 부분, 실행중인 프로그램 등을 손상시킬 수 있습니다.
- 또 이 Monolithic 커널은 커널 전체가 하나로 이루어져 있기 때문에 os 가 사용되어질 새로운 아키텍처를 위해서는 매번 다시 작성되어야만 합니다.

#### 1-2-2. Micro Kernel

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Kernel-microkernel.svg/260px-Kernel-microkernel.svg.png)

Microkernel은 시스템이 기능을 수행하기 위해 전통적인 커널을 사용하는 방식에서 최소한의 커널만을 사용해 여러 서버들이 통신하는 방식을 추구하는 디자인 접근 방법을 의미합니다. 기능 수행을 위해 system 공간은 가능한 적게 사용하고, user space는 가능한 많이 사용하는 방식을 추구합니다.

**즉, kernel의 입장에서 매번 다양한 요구사항을 반영해야만 하는데 이를 모두 하나의 kernel이 처리한다면 날이 갈수록 요구사항은 많아질테니, kernel의 크기가 매우 커지겠죠??** 

**커널의 크기가 커짐은 속도 저하를 의미하고 버그 발생 확률을 높이기 때문에 커널은 supervisor의 입장에서 최소한의 관리자 역할만 하게끔하고 다른 서브 컴포넌트들에게 커널의 책임 중 일부를 이동시키고 커널은 최소한의 책임만 가지자 라는 취지에서 나온게 microkernel 입니다.**

특정 플랫폼이나 장치를 위해 디자인된 microkernel은 정말 동작하기 위해서 필요한 코드들만 가지고 있습니다. microkernel 은 memory management, multitasking, Inter-process communication 와 같은 최소한의 OS Services를 구현하기 위한 시스템 콜 또는 primitives와 함께 하드웨에 대한 간단한 추상화의 정의로 구성되어져 있습니다.

microkernel에 의해 일반적으로 제공되어지지 않는 networking과 같은 서비스들은 server라고 불리는 user-space programs에서 구현되어져야 합니다. **Microkernel은 monolithic kernel 보다 쉽게 유지보수 할 수 있지만 Kernel의 수행 기능들이 개별 server로 넘어간 만큼 많은 system call과 context switching으로 인해 overhead가 발생할 수 있습니다.**

microkernel에서는 privileged 모드에 있어야 하는 부분만 kernel space에 존재하며 이는 Inter-Process Communication, Basic Scheduler, Scheduling Primitives, Basic memory handling, basic I/O Primitives 등을 포함합니다. 

User Space에서 수행되어야 할 중요한 부분은 Complete Scheduler, Memory Handling, File Systems, network stacks 등을 포함합니다.

Microkernel 아키텍처는 아래와 같은 장점을 설명합니다.

- 유지보수 하기 쉽다.
- 커널이 많은 일을 수행하지 않기 떄문에 새로운 소프트웨어를 테스트하기 위해 커널을 재시작 할 필요가 없다.
- 시스템 아키텍처에서 모듈성을 더 늘려 시스템을 디버깅하기 쉽게 만들 수 있으며 OS관점에서 요구사항을 더 반영하기 쉬워진다.

대부분의 Microkernels는 하나의 서버로부터 다른 서버로의 요청을 다루기 위해 message passing 시스템을 사용하며, 이는 microkernel과 함께 port 기반에서 동작합니다. 예로들어 만약 더 많은 메모리를 요구하는 요청이 전송되었다면, microkernel의 포트는 열려지고 요청이 이를 통해 전송됩니다. microkernel에 요청이 전송된 후에는 system calls와 비슷합니다.

비록 microkernels 자체는 매우 작지만, 필요한 모든 보조(auxiliary) 코드와 결합하여 이는 종종 monolithic 커널보다 더 큰 크기를 가집니다. monolithic kernel의 옹호자는 microkernel 시스템의 2계층 구조에 대해서 지적(point out) 합니다. 이는 대부분의 운영체제 시스템이 하드웨어와 직접적으로 통신할 수 없어 시스템 효율성을 챙길 수 없다고 합니다.

microkernel의 지지자(Proponents)들은 monolithic kernel의 경우 커널에서 오류가 발생했을 때 이 오류가 전체 시스템에 전파될 수 있다는 단점을 가지고 있지만, microkernel의 경우 kernel process가 충돌해도 대게 오류를 발생하는 서비스를 재시작해서 전체로 오류가 전파되는 것을 막을 수 있습니다.

microkernel의 단점은 아래와 같습니다.

- 프로그램을 실행하기 위해 수 많은 메모리가 필요하다.
- interfacing을 위한 더 많은 소프트웨어가 필요하며 이는 성능을 떨어뜨릴 수 있습니다.
- Messaging bug는 Monolithic kernel의 일회성(the one off) 사본에 비해 더 긴 여행을 해야하기 때문에 수정하기 더 어려울 수 있습니다.
- context-based 기반으로 kernel을 수행하기 때문에 부하가 많이 생길 수 있어 성능이 중요한 작업의 경우 이 커널 기반의 OS로 수행하기 어렵습니다.

**그래서 우리는 서버용 OS로 Monolithic Kernel을 사용하는 Linux 계열의 서버를 사용하는게 아닐까?**



#### 1-2-3. Monolithic Kernels vs microkernels

computer kernel이 성장함에 따라, 신뢰할 수 있는 컴퓨팅 기반의 사이즈와 취약성도 더 커집니다. 게다가 보안은 감소되고 메모리에서 실행되어야 하는 용량도 더 커집니다. 이는 가상 메모리 시스템을 도입함으로써 어느 정도 완화(mitigate)되어질 수 있지만, 모든 커널이 가상 메모리를 지원 하는 것은 아닙니다. 커널의 메모리 사용량을 줄이려면, 불필요한 코드를 줄이기 위해 수행되어져야하는 광범위한 수정이 필요합니다. 이는 수백만줄의 커널 코드간에 명백하지 않은 상호의존성 때문에 매우 어려운 작업입니다.

1990년대 초반까지 모놀리식 커널과 마이크로 커널의 다양한 단점으로 인해, monolithic kernels는 모든 OS 연구자들에 의해 쓸모없는(obsolete) 것으로 간주되었습니다. 결과적으로 microkernel 대신 monolithic kernel을 채택한 리눅스의 설계는 아주 유명한 논쟁거리가 되었습니다.

**Performance**

Monolithic Kernels는 같은 주소 공간에 모든 코드를 위치시키게 끔 디자인되었습니다. 몇몇 개발자들은 시스템의 성능을 향상시키기 위해 이러한 설계를 진행 했으며 코드가 잘 작성만 된다면 이는 극히 효율적인 kernel 설계 구조가 될 것이라고 주장합니다. monolithic 모델은 공유되어지는 커널 메모리의 사용을 통해 IPC 를 주로 사용하는 Microkernel 디자인에 비해 더 효율적인 경향이 있습니다.

microkernels의 성능은 1980년대와 1990년대 초에는 매우 느렸지만 microkernel의 성능을 실증적으로(empirically) 측정한 연구에서는 이러한 비효율의 원인을 분석하지 못했습니다. 이 데이터에 대한 설명은 커널 모드에서 사용자 모드로의 전환 빈도가 증가했기 때문에라는 가정 하에 전설에 남겨 졌습니다.

 microkernels의 낮은 성능의 이유는 다음과 같다고 1995년에 추측되어졌습니다.

1. 전체 microkernel 접근 방식의 비효율성
2. microkernel에서 구현된 특정 개념과 구현

#### 1-2-3. Hybrid Kernel

Hybrid kernels는 대부분의 상용 운영체제인 Microsoft Windows NT 3.1, NT 3.5, NT 3.51, ..., MacOS 와 같은 곳에서 사용되어집니다. 이들은 성능을 증가시키기 위해 kernel-space에 몇가지 추가 코드를 작성한 것을 제외하곤 micro kernels와 비슷합니다. 

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Kernel-hybrid.svg/260px-Kernel-hybrid.svg.png)

**이러한 타입의 커널은 monolitihc 커널의 일부 특징을 가진 micro kernels의 확장을 의미합니다. monolithic kernels와 다르게 이러한 타입의 커널은 런타임에 모듈을 로드할 수 없습니다. Hybrid Kernels는 user-space에서 사용되어질 수 있는 코드를 보다 빨리 실행하기 위해서 kernel space에 필수적이지 않은 코드를 위의 그림과 같이 넣은 구조를 의미합니다.** 

Hybrid Kernels는 monolithic 과 microkernel 설계간의 절충안(compromise) 이며 microkernel에서 발생하는 오버헤드를 줄이기 위해 커널 공간에 network stack 또는 filesystem과 같은 몇가지의 서비스를 수행하지만, 여전히 user space 에서 서버로써 커널 코드를 수행합니다.

**Monolothic Kernel은 필요시 런타임에 kernel space에 모듈을 로드해서 이를 실행하지만, Microkernel은 필요시 User Space에 server를 로드해서 실행합니다. Monolothic Kerenl은 로드한 모듈이 정상적이게 동작하지 않을 경우 특정 모듈의 에러가 전체 시스템에 전파되지만 MicroKernel의 경우 특정 서버에서 발생하는 오류로 인해 전체 시스템이 영향을 받지는 않습니다. 하지만 많은 오버헤드가 발생하구요.** 

---

## 3. Unix 및 Linux 계열의 역사. (Ubuntu, CentOS, MacOS, )

### Unix history (macOS는 Unix 계열)

![Unix history-simple.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/7/77/Unix_history-simple.svg/1920px-Unix_history-simple.svg.png)

### Linux history

https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg



### CentOs

![image-20210731143900542](/Users/user/Library/Application Support/typora-user-images/image-20210731143900542.png)



### Ubuntu

![image-20210731143945771](/Users/user/Library/Application Support/typora-user-images/image-20210731143945771.png)



---

## 4. **왜 OS는 Resource를 사용하기위해 Kernel space와 user space로 구분해서 사용할까요?**

이유는 간단하다. 커널 뿐만 아니라, Application 들도 리소스를 사용하기 위해서 OS를 사용한다. User와 Kernel 모두 동일한 리소스를 사용한다면 User에서 발생한 에러 또는 버그가 Kernel 쪽으로 전파될 수 있기 때문에 장애 전파를 Protection 하기 위해서 OS는 Resource 를 Kernel space와 User space로 나누어 사용한다.

ex) Kernel mode vs User Mode, [Memory] Kernel Space vs User Space

### User Space - [wiki](https://en.wikipedia.org/wiki/User_space)

현대 os는 보통 가상 메모리를 user space와 kernel space로 나눈다. 이러한 분리는 memory 및 hardware protection을 악의적이거나 부적절한 software 동작으로부터 보호한다.

kernel space는 privileged os kernel, kernel extendsions, device drivers 등을 실행하기 위해 예약되어지지만 user space는 application software 그리고 몇몇의 driver의 실행을 위한 메모리 공간입니다.

#### Overview

userland라고 불리는 user space는 os kernel 바깥에서 실행되는 모든 코드를 가리킵니다. userland는 보통 kernel과 상호작용하기 위해 os가 사용하는 다양한 프로그램과 라이브러리들을 의미한다. I/O 수행, file system 객체 사용,  application software 등이 사용하는 영역을 의미한다.

각 User space processs는 자신의 virtual memory를 사용하며 명시적으로 허용되지 않는한(unless) 다른 프로세스의 메모리에는 접근할 수 없다. 이것이 바로 요즘날의 주류 os에서 memory protection의 기본이고 privilege separation을 위한 block을 만드는 방법이다. 분리된 user mode는 효율적인 virtual machines를 만들기 위해 사용할 수 있다.

허가를 받는다면, process들은 다른 프로세스의 메모리 공간 일부에 자신의 메모리를 맵핑하기위해 kernel에 요청할 수 있다 이는 debuggers를 위한 경우에 사용할 수 있다. Programs는 서로 다른 여러 프로세스를 사용하여 공유 메모리 쪽에 요청을 보낼 수 있고 IPC 기법을 통해서 이를 수행할 수 있다.

아래의 그림은 userland와 kernel space를 구분한 리눅스의 구조를 나타낸다.

![image-20210801213853023](/Users/user/Library/Application Support/typora-user-images/image-20210801213853023.png)

- User mode
  - User Application
    - bash, LibreOffice, GIMP, Blender, 0 A.D., Mozilia Firfox
  - System Compoenents
    - Daemons
    - Window manager
    - Graphics
    - other libraries
  - C Standard Libraries
- Kernel Mode
  - Process Sechuduling
  - IPC Systems
  - Memory Management Subsystem
  - Virtual files subsystem
  - network subsystem
- Hardware

#### Implementation

user mode를 구현하는 가장 보편적인 방법은 os protection rings를 포함하는 kernel mode로부터 분리하는 것이다.



---

## 5. 다음 주제 생각 (Process vs Thread 관련)

### 1. Process와 Thread에 대해서 각각 조사하세요. (특히 이들이 어떠한 문제를 해결하기 위해서 나오게 되었는지에 대해서 꼭 조사하세요.)



### 2. MultiThread와 MultiProcess 의 장단점에 각각 조사하고, 각 접근 방식은 어떠한 서비스를 개발하는데 더 적절하고 왜 그런지에 대해서 조사하세요.



### 3. Blocking과 Non-Blocking 연산 수행간에 MultiThread와 MultiProcess 중 어떤 것이 더 빠르며, 어떠한 경우에 왜 빠른지에 대해서 공유 자원의 측면에서 조사하세요.











### 

