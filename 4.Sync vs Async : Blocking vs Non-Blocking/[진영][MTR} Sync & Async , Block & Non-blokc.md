# Sync & Async , Block & Non-block

----

>## Mentor's Q
>
>### 1. Synchronization, Asynchronization, Blocking, Non-Blocking은 각각 무엇인가?
>
>### 2. Tasks 를 수행하기위해 다음의 방법을 사용할 수 있을텐데 각각의 예시를 들어보라.
>
>1. Synchronous Blocking
>2. Synchronous Non-Blocking
>3. Asynchronous Blocking
>4. Asynchronous Non-Blocking
>

## 1. Synchronization, Asynchronization, Blocking, Non-Blocking은 각각 무엇인가?

![img](https://t1.daumcdn.net/cfile/tistory/99B27B3B5BC7D69604)

### 1) Synchronization 

![No Image](https://nesoy.github.io/assets/posts/20170127/Synchronous.jpg)



- 동기식 처리 모델(Synchronous processing model)은 직렬적으로 태스크(task)를 수행한다.
  즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다.

- 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 작업을 수행할 때, 서버에 데이터를 요청하고 데이터가 응답될 때까지 이후 태스크들은 블로킹(blocking, 작업 중단)된다.

- 동시에 일어난다는 뜻, 요청과 그 결과가 동시에 일어난다는 약속, 요청을 하면 시간이 얼마나 걸리던지, 요청한 자리에서 결과가 주어집니다.

#### 동기 방식의 예

![image-20210813190346124](/Users/samuel/Library/Application Support/typora-user-images/image-20210813190346124.png)

#### 동기 장/단점

동기적 방식의 장점은 코드를 파악하기 쉽고 유지보수 디버깅이 간결하다는 점

단점은, 싱글 스레드일 경우 데이터 베이스 작업이 완료되어 데이터가 오기까지 기다릴때, 애플리케이션이 유휴 상태가 된다는 점, 다른 작업을 할 수 있는 시간과 자원이 낭비

웹에서 UI를 이런식으로 구현하면 절대 안된다-> 최악의 UI제공 가능



### 2) Asynchronization

![No Image](https://nesoy.github.io/assets/posts/20170127/ASynchronous.jpg)

- 비동기식 처리 모델(Asynchronous processing model 또는 Non-Blocking processing model)은 병렬적으로 태스크를 수행한다.
- 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행한다.
  예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고(Non-Blocking) 즉시 다음 태스크를 수행한다. 이후 서버로부터 데이터가 응답되면 이벤트가 발생하고 이벤트 핸들러가 데이터를 가지고 수행할 태스크를 계속해 수행한다.

- 자바스크립트의 대부분의 DOM 이벤트와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작한다.

#### 비동기 방식의 예 (시험날의 학생과 선생)

![image-20210813190607346](/Users/samuel/Library/Application Support/typora-user-images/image-20210813190607346.png)

학생과 선생은 시험지라는 연결고리가 있지만 시험지에 행하는 행위, 즉 목적은 서로 다르다. 학생은 시험지를 푸는 역할, 선생은 채점하는 역할 서로의 행위가 다르기 때문에 둘의 작업 처리 시간은 일치하지 않고, 일치하지 않아도 된다.

#### 비동기의 장/단점

비동기의 장점은 결과가 주어지는데 시간이 오래 걸리더라도 그 시간동안 유휴에 빠지지 않고 다른 작업을 진행할 수 있으므로 자원을 효율적으로 사용 할 수 있다.

비동기의 단점은 동기 처리보다 설계가 복잡하고, dead lock이 발생할 수 있다 또한 Sync(동기식)과 반대로 Response에 대한 처리 결과를 보장받고 처리해야 되는 서비스에는 적합하지 않다.

### 3) block

![img](https://t1.daumcdn.net/cfile/tistory/2371EC4955160B8714)

I/O 작업은 유저레벨에서 직접 수행할 수 없다. 실제 I/O를 수행하는것은 커널레벨에서만 가능하다. 따라서 유저 프로세스(또는 쓰레드)는 커널에게 I/O를 요청해야한다. I/O에서 블로킹 형태의 작업은 유저 프로세스가 커널에게 I/O를 요청하는 함수를 호출하고, 커널이 작업을 완료되면 함수가 작업 결과를 반환한다.

I/O 작업이 진행되는동안 유저 프로세스는 자신의 작업을 중단한채 대기해야한다. I/O작업이 CPU자원을 거의 쓰지 않기 때문에 이런 형태의 I/O는 리소스 낭비가 심하다.

만약 여러 클라이언트가 접속하는 서버를 블로킹방식으로 구현한다고 가정해보자. I/O작업이 blocking 방식으로 구현되면 하나의 클라이언트가 I/O작업을 진행하면 해당 프로세스(또는 쓰레드)가 진행하는 작업을 중지하게 된다. 따라서 다른 클라이언트의 작업에 영향을 미치지 않게 하기 위해서 클라이언트 별로 별도의 쓰레드를 만들어 연결시켜주어야 할 것이다. 그러면 쓰레드 수는 접속자 수가 많아질 수록 엄청나게 많아지게 된다. 쓰레드가 많으면 CPU의 컨텍스트 스위칭 횟수가 증가할 것이며, 이때 사용되는 컨텍스트 스위칭 비용 때문에, 실제 작업하는 양에 비하여 훨씬 비효율적으로 동작하게 될 것이다.

#### 4. Non-Blocking

![img](https://t1.daumcdn.net/cfile/tistory/253D9E475516150118)

Blocking 방식의 비효율성을 극복하고자 만들어진 것이 Non-Blocking 방식이다. Non-Blocking은 I/O작업을 진행하는 동안 유저 프로세스의 작업을 중단시키지 않는다. 유저 프로세스가 커널에게 I/O를 요청하는 함수를 호출하면, 함수는 I/O를 요청한 다음 진행상황과 상관없이 바로 결과를 반환한다.

위 그림은 Non-Blocking 방식으로 구현된 I/O의 대표적인 사례를 잘 보여준다. 유저 프로세스는 recvfrom함수를 호출하여 커널에게 해당 소켓으로부터 데이터를 받아오고 싶다고 요청하고 있다. 커널은 이 요청에 대해서 상대방의 데이터를 전송 받아서 recvBuffer에 저장하고, 유저에게 그 내용을 복사해줘야 한다. 상대방으로 부터 데이터를 받는 중에 recvBuffer가 비어있다면 유저 프로세스가 커널에게 받아올 수 있는 정보는 없다. 따라서 recvfrom 함수는 아직 작업 진행중이란 의미로 "EWOULDBLOCK"을 리턴한다. 이 결과를 받은 유저 프로세스는 다른 작업을 진행할 수 있다. 만약 recvBuffer에 유저가 받을 수 있는 데이터가 있다면, 버퍼로 부터 데이터를 복사하여 받아온다. recvBuffer는 커널이 가지고 있는 메모리에 적재되어 있으므로 메모리간 복사가 일어나 I/O보다 훨씬 빠른 속도로 데이터를 받아올 수 있다. 이때 recvfrom함수는 빠른 속도로 읽을 수 있는 데이터를 복사해주고 복사한 데이터의 길이와 함께 반환한다. 위의 모든 반환이 I/O의 진행시간과는 관계없이 빠르게 동작하기 때문에, 유저 프로세스는 자신의 작업을 오랜시간 중지하지 않고도 I/O 처리를 수행할 수 있다.

## 2. Tasks 를 수행하기위해 다음의 방법을 사용할 수 있을텐데 각각의 예시를 들어보라. 

### Sync & Blocking (file.read(),write, )

프로그램이 데이터를 요청하고 데이터를 조회하여 가져와서 다시 줄 때까지 프로그램은 기다리게되고 응답이 될때까지 커널에서는 다음 작업을 못한다.
![img](https://media.vlpt.us/images/hotdari90/post/5e0f3d86-4287-444e-9df8-3f5817a7aaf7/2B2D8942-BBE5-46A4-98AC-415D557AF9EA.jpeg)

### Sync & Non-Blocking

프로그램이 데이터를 요청하고 커널에서는 데이터를 조회하면서 바로 제어권을 넘긴다. 그리고 프로그램에서 동기상태여서 데이터가 다되었냐고 계속 물어본다.
그리고 커널에서 데이터 조회가 되어 응답해주면 그때 프로그램측에서 받아서 처리후 다음 작업을 한다. 커널측에서 다른 작업도 데이터 조회때부터 가능해진다. 다른 작업중에 데이터가 회신되어서 넘겨줄때 그때 다시 작업을 하게 된다.
![img](https://media.vlpt.us/images/hotdari90/post/7ec831b3-ed82-44bc-af14-dd8b691a2935/AFEF9319-7C1C-44E5-912B-127357EC558E.jpeg)

### Async & Blocking

대표적인 케이스가 Node.js와 오라클, MySQL같은 조합이다. 결제
커널측에서 제어권을 넘겨주지 않아서 결구 비동기로 실행하여도 묶여있는 상태가 되어버린다.
![img](https://media.vlpt.us/images/hotdari90/post/2952849e-23d6-477b-8826-c27cea307cf1/F96CC8CC-F961-4082-9D9F-858543696D1D.jpeg)

### Async & Non-Blocking

프로그램에서 데이터를 요청하고 커널에서 바로 제어권을 반환하여
프로그램에서 다른일을 하게된다. 그리고 데이터가 반환되면
바로 프로그램에서 그 데이터를 받아서 이전에 수행하던 행위를 완료하게 된다.
![img](https://media.vlpt.us/images/hotdari90/post/bb9dbcc5-8780-4821-835a-b7750d127406/993F6A2F-0B5E-4C21-ADB6-25F06AACE2DA.jpeg)







