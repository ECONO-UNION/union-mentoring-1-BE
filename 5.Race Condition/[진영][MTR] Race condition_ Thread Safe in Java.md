# Race condition / Thread Safe in Java

----

>## Mentor's Q
>
>### 1. 멀티 스레드/프로세스 환경에서 여러 실행 흐름이 하나의 작업을 처리하는 경우 race condition이 발생할 수 있는데, 이들은 왜 일어나는 것이고 이를 해결하기 위한 기법과 장단점에 대해서 설명하시오. (3개 이상 해결 방법 서술)
>
>### 2. Java에서 여러 스레드를 사용해 하나의 연산을 처리하려고 할 때 Thread Safe 하게 연산을 수행할 수 있도록 하는 방법에 대해서 3가지 이상 조사하시오.
>
>(직접 멀티스레드를 활용해서 구현해 보는 것을 추천)

-----



[toc]

## 1. 멀티 스레드/프로세스 환경에서 여러 실행 흐름이 하나의 작업을 처리하는 경우 race condition이 발생할 수 있는데, 이들은 왜 일어나는 것이고 이를 해결하기 위한 기법과 장단점에 대해서 설명하시오. (3개 이상 해결 방법 서술)



### (1) 레이스 컨디션이란?

race condition이란 두 개 이상의 프로세스가 공통 자원을 병행적으로(concurrently) 읽거나 쓰는 동작을 할 때, 공용 데이터에 대한 접근이 어떤 순서에 따라 이루어졌는지에 따라 그 실행 결과가 같지 않고 달라지는 상황을 말한다. Race의 뜻 그대로, 간단히 말하면 경쟁하는 상태, 즉 두 개의 스레드가 하나의 자원을 놓고 서로 사용하려고 경쟁하는 상황을 말한다.

경쟁 프로세스의 경우, 세 가지 제어 문제에 직면한다. **Mutual exclusion, deadlock, starvation**이다.

**Mutual exclusion**

Race condition을 막기 위해서는 두 개 이상의 프로세스가 공용 데이터에 동시에 접근을 하는 것을 막아야 한다. 즉, 한 프로세스가 공용 데이터를 사용하고 있으면 그 자원을 사용하지 못하도록 막거나, 다른 프로세스가 그 자원을 사용하지 못하도록 막으면 이 문제를 피할 수 있다. 이것을 상호 배제(mutual exclusion)라고 부른다.

**Deadlock**

그러나 위와 같은 상호 배제를 시행하면 추가적인 제어 문제가 발생한다. 하나는 교착상태 즉 여기서 말하는 Deadlock이다. 프로세스가 각자 프로그램을 실행하기 위해 두 자원 모두에 엑세스 해야 한다고 가정할 때 프로세스는 두 자원 모두를 필요로 하므로 필요한 두 리소스를 사용하여 프로그램을 수행할 때까지 이미 소유한 리소스를 해제하지 않는다. 이러한 상황에서 두 프로세스는 교착 상태에 빠지게 되는 문제가 발생할 수 있다.

**Starvation**

이 제어 문제는 ‘기아 상태’라고도 한다. 이러한 문제는 프로세스들이 더 이상 진행을 하지 못하고 영구적으로 블록되어 있는 상태로, 시스템 자원에 대한 경쟁 도중에 발생할 수 있고 프로세스 간의 통신 과정에도 발생할 수 있는 문제이다. 두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있기 때문에 결과적으로는 아무것도 완료되지 못하는 상태가 되게 된다.

 

이렇게 race condition 인 경우에는 스레드의 실행 순서를 잘 조절해주지 않으면 이상한 상태, 비정상적인 상태가 나오게 된다. 이 문제는 항상 발생하는 것이 아니라 특정한 순서대로 수행되었을 때 발생하는 것이다. 이 문제는 디버깅을 할 때에는 전혀 보이지 않는 문제점이고, 발생 시에 모든 프로세스에 원하는 결과가 발생하는 것을 보장할 수 없으므로 후에 더욱 큰 문제를 야기할 수 있으므로 반드시 피해야 하는 상황이다.

이러한 문제가 발생하지 않도록, OS는 다른 프로세스의 의도하지 않은 간섭으로부터 각 프로세스의 데이터 및 물리적 자원을 보호해야 하며 여기에 메모리, 파일 및 I/O 장치와 관련된 내용이 포함된다.

그리고 프로세스에서 수행하는 내용과 프로세스가 생성하는 결과는, 다른 동시 프로세스의 실행 속도와 무관, 즉 기능과 결과는 서로 독립적이어야 한다.

### (2) 레이스 컨디션을 해결하기 위한 기법

**Semaphore(세마포어)**



![img](https://blog.kakaocdn.net/dn/cgvF68/btqDyMXcTWu/zhuMLl9YWBrhRkv6xkEm11/img.png)



공유된 자원의 데이터를 여러 프로세스가 접근하는 것을 막는 것이다. 또한 세마포어는 리소스의 상태를 나타내는 간단한 카운터라고 할 수 있는데, 일반적으로 비교적 긴 시간을 확보하는 리소스에 대해 이용하게 되며, 운영체제의 리소스를 경쟁적으로 사용하는 다중 프로세스에서 행동을 조정하거나 동기화 시키는 기술이다. 다시 말해서 하나의 스레드만 들어가게 할 수도 있고 여러 개의 스레드가 들어가게 할 수 있다. 이것이 뮤텍스와의 차이이다.

 

**Mutex(뮤텍스)**



![img](https://blog.kakaocdn.net/dn/KK4GG/btqDyMXcTVK/dMLkVA1QUmN3khbdFuxoF1/img.png)



공유된 자원의 데이터를 여러 스레드가 접근하는 것을 막는 방법이다. 즉, Critical Section(각 프로세스에서 공유 데이터를 엑세스하는 프로그램 코드 부분)을 가진 쓰레드들의 Running time이 서로 겹치지 않게 각각 단독으로 실행되게 하는 기술이다. 다중 프로세스들이 공유 리소스에 대한 접근을 조율하기 위해 locking과 unloking을 사용하는데, 다시 말해서 상호배제를 함으로써 두 쓰레드가 동시에 사용할 수 없다는 뜻이다.



## 2. Java에서 여러 스레드를 사용해 하나의 연산을 처리하려고 할 때 Thread Safe 하게 연산을 수행할 수 있도록 하는 방법에 대해서 3가지 이상 조사하시오.

### 1. Automic Class 사용

아래와 같이 Automic Class 를 사용하면 두 Cycle 로 나누어진 작업을 하나로
수행할 수 있게 되며, 이에 따라 위에 설명한 Thread 문제 회피 가능

```java

@WebService(name=MyCounter)
class MyCounter {
  AtomicInteger counter= new AtomicInteger( 0 );

@WebMethod
  public int getCount() {
               return counter
.incrementAndGet();
  }
}
```

### 2. Vector, HashTable, CuncurrentHashMap, String 사용

위의 Class 들은 Thread Safe 하다. 비슷한객체인 HashMap 은 Thread Safe
하지 않다. Thread Safe 가 필요한 경우 위의 변수를 활용한다.

```java
ArrayList<String>crunchifyArrayList=newArrayList<String>(); List<String>synchronizedList=Collections.synchronizedList(crunchifyArrayList); Map<String, String> crunchifyMap = new HashMap<String, String>(); 
Map<String, String> synchronizedMap = Collections.synchronizedMap(crunchifyMap);
```

### 3. Local 변수 사용

Local 변수는 Thread Safe 하다. 이유없이 Member 변수를 사용하지 않도록 한다.

### 4. Synchronized 사용

Lock & UnLock 을 통해 한시점에 하나의 Thread 만 해당 Method 혹은 Block 을 실행할 수 있도록 Thread 를 제어한다. 사용 방법은 아래와 같이 간단하지만 잘 못 사용시 시스템 성능 저하를 가지고 올 수도 있다.

```java
//Method 동기화
class MyCounter {
  private static int counter = 0;
  public static synchronized int getCount() {
    return counter++;
  }
}
//Block 동기화
private Object obj = new Object();
class MyCounter {
  private static int counter = 0;
  public static synchronized int getCount() {
   synchronized(obj) {
      return counter++;
   }
  }
}
```

### 5. Volatile

하나의 Thread 에서는 읽기/쓰기 작업을 수행하고 다른 Thread 에서는 읽기 작업만 하는 경우 최신 정보를 읽을 수 있도록 보장해 줌 . 단, 양쪽이 모두 읽기/쓰기 작업이라면
Synchronized 등 다른 방법을 사용해야 함

```java
public class SharedObject {
    public volatile int counter = 0;
}
```

