## Sync VS Async

먼저 Synchronous와 Asynchronous의 어원을 보자. Synchronous의 *Syn*는 **together**이란 뜻이고, *chrono*는 **time**이다. 따라서 Synchronous는 함께 시간을 맞춘다라는 뜻으로 해석된다. 반면에 Asynchronous는 앞에 *A*라는 접두사가 붙어 **부정**하는 형태가 되어 시간을 맞추지 않는 것이라 해석할 수 있다.

Sync와 Async를 다루려면 위 어원에서 볼 수 있듯이 함께 하는 **대상**이 누구인지, 그 대상들의 **시간**은 어떻게 다루어지는지 두 가지를 살펴봐야한다.



### Synchronization이란?

작업을 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작함을 의미한다.

![image-20210814191839800](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814191839800.png)

자신의 작업을 하다가 다른 곳에 도움을 요청했다. 

요청받은 작업은 필요한 작업을 수행하고 요청한  작업은 받은 데이터를 가지고 처리한다.



### Asynchronization이란?

시작, 종료가 일치하지 않으며, 끝나는 동시에 시작을 하지 않음을 의미한다.

![image-20210814192040807](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814192040807.png)

요청된 작업의 결과가 돌아오면 돌아온 결과에 대해 처리를 할 수도 있고 안할 수도 있다.



**Synchronous, Asynchronous 판단 방법 : 결과를 돌려주었을 때 순서와 결과에 관심이 있는지 아닌지로 판단할 수 있다.**



## Blocking vs Non-Blocking

블록킹/논블록킹은 **직접 제어할 수 없는 대상을 처리**하는 방법에 따라 나눈다. 제어권에 초점을 가진다.



### Blocking

자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 시작하는 것

![image-20210814191800310](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814191800310.png)

자신이 작업을 하다가 다른 작업이 실행되면 자신은 그동안 쉬고 있는다.

그리고 다른 직업이 끝나면 다시 직업이 이어서 진행된다.



### Non-Blocking

다른 주체의 작업에 관련없이 자신의 작업을 하는 것

다른 쪽에서 작업하는 순간 빠져나와서 자기 일을 진행하게 된다.

![image-20210814191814173](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814191814173.png)



**Blocking과 NonBlocking 판단 -> 다른 주체가 작업할 때 자신의 제어권이 있는지 없는지로 볼 수 있다.**



### Blocking/Sync

![image-20210814192501769](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814192501769.png)



Blocking 관점은 제어권 -> 다른 작업이 시작되는 동안 동작하지 않는다,.

Sync 관점은 결과의 처리 -> 결과를 반환하면 해당 작업을 바로 처리하게 된다.



### Non-Blocking/Sync

![image-20210814192656757](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814192656757.png)

NonBlocin은 다른 작업이 있어도 자신의 제어권을 가지고 일을 함

그러나 동기는 결과에 관심있다. 그래서 중간중간 결과 나왔는지 계속해서 물어본다.

결과가 끝나서 겨로가를 받을 수 있으면 해당 결과를 가지고 와서 업무를 처리한다.



### Blocking/Async

![image-20210814193004583](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814193004583.png)

Blocking이므로 자신의 작업에 대한 제어권 x

Async이기 때문에 결과 바로 처리 x



### NonBlocking/Async

![image-20210814193122209](C:\Users\홍동건\AppData\Roaming\Typora\typora-user-images\image-20210814193122209.png)

NonBlocin은 다른 작업이 있어도 자신의 제어권을 가지고 일을 함

각자 작업 처리

Async이기 때문에 결과 바로 처리 x -> 다른 작업에서 끝난 결과를 바로 처리하지 않고 자신일이 끝나게 되면 그때야 처리