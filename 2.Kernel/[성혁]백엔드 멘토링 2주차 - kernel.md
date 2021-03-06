**왜 OS는 Resource를 사용하기위해 Kernel space와 user space로 구분해서 사용할까요?**



# 커널



![img](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/Kernel_Layout.svg/220px-Kernel_Layout.svg.png)



커널은 컴퓨터 운영체제의 주요 구성 요소이며, 컴퓨터 하드웨어와 프로세스를 잇는 핵심 인터페이스이다. 시스템의 모든 것(하드웨어의 모든 주요 기능)을 완벽하게 제어한다.

항상 메모리 내부에 상주하는 운영체제 코드의 일부분이다. 그리고 하드웨어와 소프트웨어 구성요소의 상호작용을 용이하게 한다. 대부분의 시스템에서 커널은 부트로더 다음으로 가장 첫 번째로 로드되는 프로그램 중 하나이다. 

커널의 중요한 코드는 보통 메모리의 별도의 영역에 로드되며, 이 영역은 애플리케이션 소프트웨어나 기타 운영체제의 덜 중요한 부분에 의한 액세스로부터 보호된다.



### kernel space 에서 하는 일은?

* kernel space는 커널(운영체제의 코어)이 실행되고 서비스를 제공하는 곳
* 사용자가 간섭할 수 없는 곳
* 프로세스 실행, 하드 디스크와 같은 하드웨어 장치 관리, 인터럽트 핸들링과 같은 작업을 수행



### user space 에서 하는 일은?

* user space는 사용자 프로세스가 실행되는 시스템 메모리의 부분
* 브라우저, 워드 프로세서 또는 오디오, 비디오 플레이어와 같은 응용 프로그램은 분리된 메모리의 영역인 user space을 사용한다.



### kernel space와 user space를 구분해서 사용하는 이유?

* 사용자 데이터와 커널 데이터가 서로 간섭하고, 불안정과 속도 저하를 유발하는 것을 방지할 뿐만 아니라 오작동하는 응용 프로그램이 다른 응용 프로그램에 영향을 미치거나 전체 운영체제와 충돌하는 것을 예방한다.

* 

커널의 인터페이스는 낮은 수준의 추상화된 계층이다. 한 프로세스가 커널로부터 서비스를 요청할 때, 대게 래퍼 함수를 통해 시스템콜을 호출해야만 한다.



### Monolithic kernel vs Microkernel

* Monolithic kernel은 주로 속도를 위해 supervisor mode에서 동작하는 CPU와 함께 단일 주소 공간에서 완전하게 실행된다.
* Micro kernel은 대부분의 서비스를 user space에서 실행하지만 사용자 프로세스와 마찬가지로 주로 탄력성과 모듈성을 위해 실행한다.



커널은 실행 중인 많은 프로그램 중 프로세서 또는 프로세서에 할당해야 하는 프로그램을 언제든지 결정할 책임이 있다.



### 커널이 하는 일

* RAM이 한정적인 상황에서 커널은 각 프로세스가 사용할 수 있는 메모리를 결정하고 사용 가능한 메모리가 충분하지 않을 때 수행할 작업을 결정한다.
* 커널은 적절한 장치에 I/O를 수행하기 위해 응용 프로그램으로부터의 요청을 할당하고 장치를 사용하는 것에 대한 편리한 방법을 제공한다.
* 