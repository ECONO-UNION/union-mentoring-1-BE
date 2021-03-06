### 아래의 그림과 같이 Multi-Core Processor에서 각각의 구성요소들이 하는 역할에 대해서 조사하고 Multi-core Processor는 Single-core Processor보다 무조건적으로 두 배 빠른 처리 속도를 가지는가? (물론 단편적인 답은 없다.)

=> 직접 그림을 그려보시는걸 추천!! (잊어버릴 수가 없다.)



![image](/Users/sunghyuki/Desktop/Econovation/유니온 멘토링/image.png)



=> 단순히 코어의 개수만을 프로세서의 성능의 지표로 사용하기는 어렵다. 클럭 속도(CPU가 초당 실행하는 사이클 수)를 고려하여야 하고 클럭당 성능(IPC, 클럭당 명령어 처리 개수)도 고려해야 한다. 또한 아키텍처(코어수, CPU 캐시 및 전력 소비량)에 따라서도 달라질 수 있다.

어떤 상황에서 어떻게 다른지는 궁금해서 좀 더 찾아보겠습니다..!



#### 프로세서

* 프로세서는 컴퓨터를 작동시키는 기본 명령을 해석하고 수행하는 역할을 한다
* 프로세서는 전체적인 컴퓨터 성능에 상당한 영향을 미치고 대부분의 컴퓨터 작업을 관리한다
* 소프트웨어의 신호를 받아 다른 하드웨어 부분으로 신호를 보내는 제어장치(Control Unit)와 사칙연산과 논리연산과 같은 연산을 담당하는 연산장치(ALU)으로 구성
* 컴퓨터가 하는 일 모든 것을 총괄하는 것이 CPU라면, CPU를 보조하며 연산, 제어의 핵심부분을 담당하는 것이 프로세서
* 컴퓨터 운영을 위해 기본적인 명령어들을 처리하고 반응하기 위한 논리회로 (https://donghoson.tistory.com/14)



##### 동시성과 병렬성

![스크린샷 2021-07-24 오후 12.39.07](/Users/sunghyuki/Desktop/스크린샷 2021-07-24 오후 12.39.07.png)

* 동시성은 프로세스 하나가 여러 작업을 돌아가면서 일부분씩 일을 수행하는 것(문맥 교환)
* 병렬성은 프로세스 하나에 코어가 여러 개 달려서 코어가 각각의 작업들을 나눠서 수행하는 것

자동차로 비유했을 때 프로세서는 엔진, 코어는 피스톤



#### CPU(Central Processing Unit, 중앙 처리 장치)

* 컴퓨터의 뇌라고 불린다
* 프로그램의 명령어를 해석하여 데이터를 연산/처리 하는 부분
* 논리적 사고를 하는 부분으로 기억, 연산, 제어의 역할을 담당



#### 코어(Core)

* 각종 연산을 담당하는 CPU의 핵심요소



#### 캐시(Cache)

* 속도가 빠른 장치(코어)와 느린 장치 사이(메모리)에서 속도차에 따른 **병목 현상**을 줄이기 위한 범용 메모리
* L1, L2, L3 캐시 메모리, 여기서  L은 'Level'
* L1 캐시는 일반적으로 CPU 칩안에 내장되어 데이터 사용/참조에 가장 먼저 사용, 여기서 데이터를 찾지 못하면, 이제 L2 캐시 메모리로 넘어간다.
* https://12bme.tistory.com/402



