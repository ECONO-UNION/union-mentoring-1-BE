# Memory Management

### 1. 과거 우리는 메인 메모리에 여러 프로세스를 로드하기위해 메모리를 파티션으로 나누어 프로세스를 로드하는 방식인 파티셔닝 기법을 사용하였다. 그렇다면 파티셔닝(동적, 정적) 기법으로 프로세스에 메모리를 할당할 때 생기는 문제점은 무엇이고 어떤 해결방법이 있는가?

### Fixed Partitioning

**Fixed Partitioning이란 메인 메모리를 파티션이라고 불리는 서로 다른 크기의 고정된 물리적 단위로 나누는 방법이다.** 당연히 os는 실행되어야 하기 때문에 첫 번째 파티션에 있겠죠? fixed partitioning는 다음과 같은 요구사항을 충족해야 한다. 파티션은 서로 겹쳐질 수 없으며, 프로세스는 실행을 위해 파티션에 연속적으로 존재해야 한다.

Fixed Partitioning이란 메모리를 관리하기 위한 매우 쉬운 방법이라는 장점이 있지만 그보다도 더 많은 단점이 존재한다.

1. Internal Fragmentation

   ![image-20210915193641616](/Users/user/Library/Application Support/typora-user-images/image-20210915193641616.png)

   process가 사용하는 메모리가 파티션의 크기보다 작다면 파티션의 크기와 프로세스가 요구하는 메모리 사이에는 사용되지 않는 유휴 메모리 자원이 생길 것이고 이러한 것들을 Internal Fragmentation 이라고 한다.

2. External Fragmentation

   ![image-20210915193718774](/Users/user/Library/Application Support/typora-user-images/image-20210915193718774.png)

   새로운 프로세스를 로드할 수 없을 정도의 여러 파티션들 사이에 위치한 조그만 공간을 의미합니다.

3. Limitation on the size of the process

   프로세스는 파티션내의 연속적인 메모리 공간을 사용해야 한다는 가정 때문에 만약 프로세스가 요구하는 메모리 용량이 파티션의 용량보다 클 경우 프로세스를 로드하지 못하는 일이 발생합니다. 그렇다고 해서 파티션의 용량을 늘린다면 Fragmentation Problem이 더 심각해지겠죠?

4. Degree of multiprogramming is less

   프로세스의 크기에 따라 파티션의 크기를 변경할 수 없기 때문에 다중 프로그래밍의 정도 즉, 한 시점에 메모리에 올라갈 수 있는 프로세스의 수가 매우 적어집니다.

**CPU는 매우 발전해서 단위 시간당 처리할 수 있는 프로세스 및 작업의 수를 높였는데, 메모리 및 파티션의 사이즈 때문에 병목이 발생해서 특정 속도 이상으로 작업을 처리할 수 없는 상황이 발생.**

### Dynamic Partitioning

**Dynamic Partitioning이란 프로그램이 시작하기 전에 이를 담을 파티션의 크기를 정해놓고 시작하는게 아니라 프로세스 로딩시 메모리 용량 즉, 파티션의 크기를 결정하는 방식을 의미한다.** 

장점

1. No Internal Fragmentation

   프로세스의 메모리 요구량에 따라 파티션의 크기가 결정되는 것이기 때문에 파티션에서 사용되지 않을 메모리 즉, Internal Frgamentation이 발생할 확률이 없다.

2. No Limitation on the size of the process

   파티션의 사이즈는 프로세스의 메모리 요구량에 따라 동적으로 변화할 수 있기 때문에 메모리에 충분한 공간만 남아 있다면 프로세스의 사이즈는 제한되지 않는다.

3. Degree of multiprogramming is dynamic

   Internal Fragmentation이 없어지기 때문에 사용할 수 있는 가용 메모리가 많아지고 이는 자연스레 동시에 많은 프로세스가 메모리에 자유롭게 올라가고 내려갈 수 있게 만들어 준다. 결론적으로는 멀티 프로그래밍의 정도가 동적으로 변화할 수 있다는 것을 의미한다.

단점

1. External Fragmentation

   아무리 동적으로 파티션의 크기를 정의한다고 해도, 프로세스는 연속적인 메모리 공간이 필요하기 때문에 파티션들 사이에 남아있는 공간이 메모리에 올라갈 프로세스의 메모리 요구량보다 작다면 해당 메모리는 유휴상태가 된다. 우리는 이걸 External Framgentation 이라고 한다.

### The Solution of Partitioning Problem

##### Compaction

Fixed Partitioning 뿐만 아니라 Dynamic Partitioning도 해결하지 못한 문제는 External Framgentation이다. External Fragmentation을 해결하기 위한 대표적인 방법은 Compaction이며 이는 프로세스가 사용하고 있는 메모리 공간 즉, 파티션을 한 쪽으로 몰아 파티션들 사이에서 놀고 있는 External Frament들도 한 쪽으로 모으는 작업을 의미한다.

Compaction을 통해 External Fragment을 없앰으로써 메모리의 모든 External Fragments 크기의 합 만큼 새로운 공간에 새로운 프로세스들을 할당할 수 있게되어 메모리 사용량이 늘어나게 된다. 그럼 자연스레 Dgree of multiprogramming도 늘어 나겠지요?

하지만 Compaction의 경우 효율이 좋지 않기 때문에 권장하는 방법은 아니다. 메모리 내의 여러 곳에 위치한 모든 External Fragments이 한 곳으로 모여지기 때문에 이 과정에서 엄청난 시간이 소모되고 무엇보다 CPU는 이 Compaction 작업 시간동안 유휴 상태에 위치하게 됩니다.

#### Paging(2번 주제에서 다룸)

---

### 2. 단편화를 해결하는 방법 중 하나인 Compaction은 많은 오버헤드를 발생시킨다고 알려져있다. Compaction과 달리 많은 오버헤드를 발생시키지 않고 외부 단편화를 극적으로 없애는 기법은 Paging 기법인데 이 기법이 나오게 된 이유와, 원리, 장단점에 대해서 설명해주세요.

Paging기법이 나오게 된 이유는 External Fragmentation 문제를 해결하기 위해서 나오게 되었습니다. Compaction도 좋은 Solution이지만 External Fragments와 Serveral Partions를 한 장소로 모아야 하는 작업이 수 많은 오버헤드를 발생시키기 때문에 현재 잘 사용되지 않습니다.

Paging 기법은 현재 서버용 OS 부터 모바일 OS 까지 현존하는 대부분의 운영체제에서 사용하는 메모리 관리 기법입니다. 

**Paging 기법이 메모리를 관리하는 방식은 다음과 같다.**

1. Memory를 Physical Memory와 Logical Memory로 나눈다.
2. Physical Memory는 같은 크기의 메모리 블록인 Frame으로 나누고, Logical Memory는 같은 크기의 메모리 블록인 Page로 나눈다.
3. Page Table을 사용해서 Logical Memory의 메모리 블록인 Page와 Physical Memory의 메모리 블록인 Frame을 맵핑한다.

**즉, 프로세스가 로딩시 필요한 메모리를 계산해 이를 같은 크기의 메모리 불록인 Page들의 집합인 Logical Memory로 구성하고 구성된 Logical Memory의 개별 Page들을 Page Table을 사용해서 Physical Memory의 프레임에 맵핑한다. 프로세스의 입장에선 Logical Memory에 연속적으로 데이터가 저장된 것 처럼 보이지만 실제 Physical Memory는 External Fragmentation을 없애기 위해 남는 프레임 공간에 Page를 맵핑한다. ** 

![image-20210915212845831](/Users/user/Library/Application Support/typora-user-images/image-20210915212845831.png)

즉, 우리는 큰 부하 없이 External Fragmentation을 해결할 수 있지만 (프로세스 메모리 사용량 / page 단위) 가 항상 딱 떨어질 수 없기 때문에 Internal Fragmentation이 발생할 수 밖에 없다. 이를 해결하기 위한 방법이 Memory Segmentation이다.

---

### 3. 메모리를 물리적이 아닌 논리적 내용의 단위인 세그먼트로 분할하여 메모리를 관리하는 기법인 Memory Segmentation에 대해서 알려주시고, Paging 기법이랑 비교해주세요

#### What is the memory segmentation

프로세스의 메모리를 Consecutive Logical Memory 라고 가정하고 이를 Segment 라고 불리는 논리적 단위의 가변 크기의 블록으로 나누어서 Segment 테이블을 통해 Physical Memory로 맵핑시켜서 메모리를 관리하는 방법이다.

![image-20210916204141191](/Users/user/Library/Application Support/typora-user-images/image-20210916204141191.png)

Segment Map Table은 크게 아래와 같은 4가지 요소로 구성된다.

- Limit : the length of the segment
- Base : the base address (physical memory) of the segment
- Offset : The Relative Location from Base Address
- Segment Number : The identity of Segment

Limit과 2^Offset 의 비교를 통해 생성한 segment가 유효한지 아닌지 판단한다.
`if Limit > 2^Offset : Invalid; else : Valid;`

#### **Advantages of Segmentation**

1. No internal fragmentation
2. Average Segment Size is larger than the actual page size.
3. Less overhead (Cache Hit Rate를 매우 높일 수 있겠다.)
4. It is easier to relocate segments than entire address space.
5. The segment table is of lesser size as compared to the page table in paging.

#### **Disadvantages**

1. It can have external fragmentation.
2. it is difficult to allocate contiguous memory to variable sized partition.
3. Costly memory management algorithms.

#### Paging VS Segmentation

| Criteria                                 | Paging                                | Segmentation                          |
| ---------------------------------------- | ------------------------------------- | ------------------------------------- |
| memory is contiguous?                    | 비연속적인 Physical Memory Allocation | 비연속적인 Physical Memory Allocation |
| unit is fixed?                           | fixed size page                       | variable size segments                |
| which is reponsible?                     | OS (fixed)                            | Compiler (Unit : Function, Class, ..) |
| who is it for?                           | OS                                    | User                                  |
| which type Fragmentation is occured?     | internal Fragmentation                | External Fragmentation                |
| what is the information which table has? | frame number, flag bits, ...          | base address, protection bits, ..     |

---

### 4. 가상 메모리는 무엇이고 왜 나오게 되었나요? 그리고 이를 통해 우리는 무엇을 할 수 있을까요?

**가상 메모리(Virtual Memory)란 프로세스에게 실제 메모리가 아닌 가상 메모리를 부여하여 작업을 처리하는 방법이다.** 프로세스에게 실제 메모리가 아닌 가상 메모리를 할당해 줌으로써 OS는 실제 Physical Memory 보다 더 큰 영역의 메모리를 여러 프로세스에게 제공할 수 있다.

가상 주소 공간은 메모리 관리 장치(Memory Management Unit, MMU)에 의해서 물리 주소로 변환된다. 이 덕분에 프로그래머는 가상 주소 공간에서 프로그램을 짜게 되어 프로그램이나 데이터가 Physical Memory 상에 어떻게 존재하는지를 의식할 필요가 없어지기 떄문에 구현이 더 간단해진다. 현재 상용되어 있는 대부분의 운영체제에서 메모리 관리 기법으로 Virtual Memory를 채택해 사용중에 있다.

가상 메모리를 사용함으로써 얻을 수 있는 가장 큰 이점은 MultiProgramming 의 정도를 매우 높일 수 있게 된다는 것이다. 즉, 고정된 크기의 물리 메모리에 많은 수의 프로세스를 로드할 수 있으며 여러 프로그램을 동시에 실행할 수 있게 되니 컴퓨터의 초당 연산 속도도 당연히 매우 빨라지게 된다. Virtual Memory가 나오기 이전에는 메모리 용량이 컴퓨터 전체 성능의 병목이었다. CPU는 매우 발전하는 반면에 메모리에 올릴 수 있는 프로그램의 사이즈는 매번 Physical Memory 크기로 정해져있어서 전체 컴퓨터의 속도가 매우 느렸었다. 하지만 Virtual Memory가 나온 후 컴퓨터 전체 성능이 매우 늘어나게 되었다.

---

### 5. 가상 메모리 시스템에서 자주 사용되는 기법 중 하나인 Demand Paging은 무엇이고 왜 사용하게 되었으며 Demand Paging의 핵심 알고리즘인 페이지 교체 알고리즘에 대해서 장단점을 포함해 소개해주세요.

