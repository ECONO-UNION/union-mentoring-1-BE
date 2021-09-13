## 1. The history of Process and Thread

### The History of Process

1960년대 초반에, 컴퓨터를 제어하는 소프트웨어는 IBSYS와 같은 모니터를 제어하는 소프트웨어로부터 실행을 제어하는 소프트웨어(executing control software)로 진화했습니다. 시간이 지남에 따라 컴퓨터 하드웨어는 더 빨라졌지만 소프트웨어를 통한 연산 시간과 비용은 여전히 비쌌고 이들은 발전된 하드웨어를 잘 사용하지도 못했습니다. 이러한 환경은 multiprogramming을 필요하게끔 만들었습니다. 

처음에는 단일 프로세서 컴퓨터 아키텍처의 결과로 하나 이상의 프로그램이 단일 프로세서에서 실행되었으며 희소하고 제한된 하드웨어 리소스를 공유하였습니다. 후에 2개 이상의 processor를 가진 시스템이 나온 뒤 병렬적으로 여러 프로그램이 동시에 수행되어졌습니다.

Program은 Processor를 위한 일련의 명령어로 구성되어 있습니다. 단일 프로세서는 동시에 오직 하나의 명령만 실행할 수 있었고 동시에 하나 이상의 프로그램을 실행하는 것은 불가능 했습니다. 프로그램은 input device와 같은 큰 지연을 가진 자원을 필요로 하거나 출력을 프린터로 보내는 것과 같은 일부 느린 작업을 필요로 했고 이는 processor의 fever time을 떨어뜨렸습니다.

processor를 매번 바쁘게 만들기 위해서 오래 기다려야하는 프로그램의 실행은 일시 중단시키고 os가 다른 프로그램을 실행시키도록 processor를 바꾸어주어야 했습니다. 이러한 작업들은 사용자들에게 프로그램이 동시에 실행이 되어지는 것 처럼 보여졌습니다.

**머지 않아 program이라는 용어는 "executing program and its context" 라는 용어로 확장되어졌고 사람들은 이들은 Process라고 부르게 되었습니다.**

time-sharing, computer networks, multiple-CPU shared memory computers 출현으로 오래된 multiprogramming 개념은 진정한 multitasking, multiprocessing 그리고 나중에는 multithreading으로 자리를 잡았습니다.

**즉, Process는 I/O와 같이 많은 병목을 발생시키는 작업을 포함하는 Program의 효율을 높이고자 시작된 multiprogramming 발전 과정에서 "실행하고 있는 프로그램과 이들의 문맥(executing program and it's context)"을 표현하기 위한 단어이다.**



### The History of Thread

- 1965년 버클리 시분할 시스템에서 공유 변수나 세마포 혹은 그와 유사한 방법들을 통해 서로 상호작용하는 프로세스들이 존재했다. 이때의 프로세스가 스레드 개념의 시작이다.

- 그 후 1970년대에 Multics에서 한 개의 무거운 프로세스안에 멀티 스택을 이용해 백그라운 컴파일을 돕는 기능을 수행하는 오늘날의 스레드와 가장 유사한 개념을 가진 Processing 단위가 등장했다
- 그 후 유닉스가 나왔으며 유닉스의 프로세스는 멀틱스 프로세스 디자인에서 착안되었다. 유닉스는 프로세스에 가상주소 공간을 추가해 프로세스간의 Isolation을 이끌었으며 IPC를 추구했다.
- 그 후 유닉스 사용자들은 메모리를 공유할 수 있는 예전 방식의 프로세스를 그리워 했고 이게 스레드의 발명을 이끌었다. microkernels이 나올 때 까지 기존 방식의 프로세스를 heavyweight Process라고 불렸으며 새로운 방식의 프로세스를 lightweight process라고 명명했다. 이 lightweight process가 발전되어 오늘날의 스레드가 되었다.

**즉, 스레드는 multiprogramming에서 병렬 처리를 위해 서로 메모리를 공유하는 프로세스인 lightweight process로부터 탄생되었다. LWP가 Hevyweight Process 내에 들어가서, Process가 점유하는 자원을 공유하며 연산을 처리하는게 오늘날의 Thread가 되었다.**



### 결론적으로 Thread 와 Process 모두 multiprogramming을 통해 리소스를 효율적으로 사용하는데서 유래되었다.



---

## 2. What is the Process and Thread

### Process

**Process는 하나 이상의 스레드를 실행하는 executing program and its context를 의미한다. OS에 따르면, 프로세스는 명령어를 동시에 실행하는 여러 스레드의 실행으로 이루어져있다. 컴퓨터 프로그램은 일반적으로 디스크의 파일에 저장된 명령어의 집합이지만, process는 디스크로부터 메모리로 로드된 이러한 명령어들의 실행을 의미합니다.**

multitasking은 여러 프로세스가 cpu와 다른 시스템 자원을 공유하는 것을 가능하게 해줍니다. 각 CPU(core)는 동시에 하나의 작업만 실행할 수 있습니다. 그러나 multitasking은 각 task가 끝나는 것을 기다리지 않고 실행하기 위해서 task들 사이에서 switch 합니다. 

 멀티태스킹의 일반적인 형태는 CPU의 time sharing에 의해서 제공되어집니다. CPU time sharing은 사용자의 프로세스와 스레드 심지어는 독립적인 커널 작업의 실행을 interleaving하는 방법입니다. 비록 후자의 특징은 리눅스와 같은 선점형 커널에서만 실현 가능합니다.(feasible) 

선점은 CPU bound 프로세스와 관련하여 더 높은 우선 순위가 부여되는 대화식 프로세스에 중요한 부작용이 있습니다. 그러므로 사용자는 키나 마우스를 누르는 것과 같은 간단한 작업을 할 때 컴퓨팅 자원이 즉시 할당됩니다. 또한 비디오 및 음악 재생과 같은 응용 프로그램에는 일종의 실시간 우선 순위가 부여되어 다른 낮은 우선 순위 프로세스를 선점합니다.

시분할 시스템에서 Context switches는 빠르게 수행되어지며 이는 동일한 프로세서에서 여러 개의 프로세스가 실행되어지는 것 처럼 보여집니다. 이러한 여러 프로세스의 동시 실행을 concurrency 라고 부릅니다.

보안 및 안정성을 위해, 대부분의 현대 운영체제는 독립적인 프로세스간에 직접적인 상호작용을 막고, IPC 기능을 제공하여 엄격히 중제 및 제어합니다.

### Process Representation

일반적으로, process는 다음의 자원으로 이루어져 있습니다.

- 프로그램과 관련된 실행가능한 기계어 코드의 이미지
- 실행가능한 코드, 프로세스 특정 데이터, call stack, heap 등을 포함하는 메모리를 가지고 있습니다.
- file descriptor나 handles, datasource, sink와 같은 프로세스에 할당되는 리소스의 OS descriptor를 가지고 있습니다.
- process owner, permissons와 같은 보안 속성
- 레지스터 내용 및 물리적 메모리 주소와 같은 Processor 상태(context)

OS는 process control blocks(PCB)라고 부르는 자료 구조에 실행중인 프로세스의 정보의 대부분을 저장합니다. 일반적으로 프로세서 상태인 리소스의 하위 집합은 자식 프로세스 또는 스레드를 지원하는 운영체제의 프로세스의 스레드와 연관되어질 수 있습니다.

OS는 프로세스들이 서로 간섭하고 시스템 오류(deadlock or thrashing)를 일으킬 가능성이 적도록 하기 위해서 별도로 분리하고 필요한 리소스를 할당합니다. OS는 프로세스들이 안전하고 예측 가능한 방법으로 상호작용 하기 위하여 Inter Process Communication(IPC)와 같은 방법을 제공해줍니다.

**Monolithic kernel, microkernel과 비슷한 논점이죠?**

### Multitasking and Process management

multitasking os는 많은 프로세스가 동시에 실행하는 것을 나타내기 위해 프로세스들간에 switch 합니다. 단일 프로세스를 주 프로그램과 연관시키고 자식 프로세스를 비동기 서브루틴처럼 작동하는 파생 병렬 프로세스와 연관시는 것이 일반적입니다.

Process들은 종종 embedded OS에서 "tasks" 라고 불려지며 "something that takes up time"을 의미합니다.

프로세스가 기다려야만 하는 특정 작업을 요청한다면 이는 block 되었다고 표현합니다. process가 blocked state에 있다면 disk로 swapping 되어질 수 있습니다. 이러한 경우 가상 메모리 시스템에서 process의 메모리 위치는 physical memory 보다 disk에 있을 확률이 더 높습니다. 

가상 메모리 시스템에서는 프로세스가 block 되지 않아도 일부가 최근에 사용되어지지 않았다면 이들도 disk로 swapping 되어질 수 있습니다. 즉, 실행하고 있는 프로그램과 데이터의 모든 부분이 프로세스의 실행을 위해 모두 물리 메모리에 위치해야하는 것은 아니라는 말입니다.

### Process States

multitasking을 허용하는 OS Kernel은 프로세스가 certain states를 가져야합니다. 이러한 states들의 이름은 표준화되어있지 않지만 비슷한 기능을 가집니다.

![image-20210807105515804](/Users/user/Library/Application Support/typora-user-images/image-20210807105515804.png)

- 프로세스는 디스크로부터 메모리로 로드되어 "Created"됩니다. 그 후 process scheduler는 "wating" 상태를 할당합니다.
- process가 "waiting" 상태에 있는 동안, 이는 scheduler가 소위 말하는 context switch 를 진행하도록 기다립니다. context switch란 process를 processor에게 로드시키는 동작이고 process의 상태를 "running" 으로 변화시킵니다. 기존에 processor에서 동작하는 "running" 프로세서는 "waiting" 상태로 저장되어집니다.
- 만약 "running" 상태의 프로세스가 I/O와 같은 자원을 기다린다면 scheduler는 이 프로세스에게 "blocked" 상태를 할당시킬 것입니다. "blocked" 상태에 있는 process가 더이상 기다릴 필요가 없다면 "waiting" 상태로 변경되어질 것입니다.
- process가 실행을 끝내거나 OS에 의해 종료되어지거나 더이상 필요가 없다면, processs는 메모리로부터 제거되어지고 "terminated" 상태로 변할것입니다.



### Memory layout for a typical process

![image-20210807110244349](/Users/user/Library/Application Support/typora-user-images/image-20210807110244349.png)

- **Text**: Text영역은 Code segment라고도 불리우며 이는 프로그램의 실행가능한 명령어들을 포함하고 있다. 

- **Data**: Data 영역은 "Initialized data segment"의 약어(shorthand) 입니다. 프로그램의 가상 주소 공간의 이 부분은 프로그래머에 의해 초기화되어지는 global, static variables를 포함한다.

- **BSS**: BSS 영역은 "Uninitialized data segment" 라고 합니다. 이 영역에 있는 데이터는 프로그램이 실행되기 전에 OS kernel에 의해서 0으로 초기화되어집니다. 일반적으로 이 곤간은 데이터 세그먼트의 끝에서 시작하고 0으로 초기화되거나 소스 코드에서 명시적 초기화가 없는 모든 전역 및 정적 변수를 포함합니다. (i.e `statc int i`)

- **Stack**: Stack 영역은 Program stack을 포함합니다. 이는 LIFO 구조를 가지며 프로세스에 할당되는 메모리 주소의 상단에 위치하며 OS Kernel 공간 아래에 위치합니다. **이 공간은 프로그램에서 함수 호출을 위해 필요한 모든 데이터를 저장하기위해 사용되어집니다. 하나의 함수 호출을 위해 밀어넣어진 값들의 집합을 stack frame 이라고 하며, 이는 automatic 변수와 호출자의 반환 주소로 구성되어집니다.**

  stack pointer register는 stack의 top을 가리키며, 이는 값들이 push 되어지거나 pop 되어질 때 매번 조정되어집니다. 만약 stack pointer가 heap pointer를 만날 때 가용 공간이 고갈되었다는 것을 의미합니다.

- **Heap:** Heap은 동적 메모리 할당이 발생하는 영역을 의미합니다. 이 영역은 변수와 같이 런타임에 사이즈가 알려질 수 있는 변수를 위해 프로그래머에 의해서 요청되어지는 메모리 공간이며 프로그램이 시작되기 전에 컴파일러에 의해서 결정되어질 수 있는 정적인 공간이 아닙니다.



### Thread

Computer Science에서, Thread 실행은 OS의 일부인 scheduler에서 독립적으로 관리되어지는 명령어의 가장 작은 Sequence라고 할 수 있습니다. OS 마다 Process와 Thread의 구현은 다르지만, 대부분의 경우 Thread는 Process의 구성요소입니다. Process에서 여러 Thread들은 메모리와 같은 자원을 공유해서 동시에 실행하지만 프로세스들은 이러한 자원을 공유하지 않습니다.

특히, 하나의 프로세스의 스레드들은 executable code segments(text), 동적으로 할당 되어지는 변수(heap), 전역 변수segments (data, BSS) 를 공유합니다.

### 멀티 스레딩이란 무엇인가요?

**하나의 프로세스를 다수의 실행 단위 즉, 여러개의 스레드로 구분**하여 프로세스에게 할당된 자원을 공유해 **병렬 처리 능력을 향상** 시키는 것을 멀티스레딩이라고 합니다.

#### 스택을 스레드마다 독립적으로 할당하는 이유

스택은 함수의 인자, 복귀 주소, 지역 변수 등을 저장하기 위해 사용되는 메모리 공간이므로 **스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하다는 것이고 이는 독립적인 실행 흐름이 추가**되는 것이기 때문에 프로세스를 여러 실행 흐름으로 만들기 위해서는 스레드에 스택을 독립적으로 할당해야 합니다.

#### PC Register를 스레드마다 독립적으로 할당하는 이유

PC 값은 다음에 실행될 명령어의 주소를 나타내므로 **프로세스의 개별 실행 흐름인 스레드가 독립적으로 명령어들을 실행하기 위해선 스레드에 PC 레지스터가 독립적으로 할당**되어야 합니다.



---

### 1. Synchronization, Asynchronization, Blocking, Non-Blocking은 각각 무엇인가?



### 2. Tasks 를 수행하기위해 다음의 방법을 사용할 수 있을텐데 각각의 예시를 들어보라.

1. Synchronous Blocking
2. Synchronous Non-Blocking
3. Asynchronous Blocking
4. Asynchronous Non-Blocking



### 3. MultiThread 환경에서 Blocking과 Non-Blocking 을 활용해 Tasks를 각각 처리한다면 어떤 것이 더 빠르겠는가? 공유자원의 측면에서 이유를 제시하시오.





