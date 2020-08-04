# Dispatch Queue

앱 작업을 비동기식으로 수행할 수 있는 GCD 추상화 계층이다. 

작업은 항상 queue에 추가된 순서대로 실행된다.

---



## **Serial, Concurrent**

### **serial queue (하나의 스레드)**

serial dispatch queue는 한 번에 하나의 작업만 수행한다. 

race condition이 발생하지 않도록 특정 값이나 자원에 대한 액세스를 동기화 ㅇ ㅇ하는데 사용된다.

``` swift
let serialQueue = DispatchQueue(label: "serial.queue")
```





### **concurrent queue (여러 개의 스레드)**

concurrent queue는 동시에 여러 가지 작업을 수행할 수 있다. 들어오는 대로 실행되지만, 끝나는 순서는 알 수 없다.

작업은 다른 스레드에서 실행될 것이며, 시스템 상황에 따라서 개수가 결정된다.

``` swift
let concurrentQueue = DispatchQueue(label: "concurrent.queue", attributes: .concurrent)
```





## **Synchronous, Asynchronous**

DispatchQueue의 작업은 sync, async하게 사용할 수 있다. 

가장 큰 차이점은 작업을 생성할 때이다.



### **Synchronous**

작업이 시작하면 스레드가 작업이 끝날 때까지 block 상태가 된다.





### **Asynchronous**

작업이 시작과 동시에 반환되며, 스레드는 block 상태가 되지 않는다.





### **main.sync를 사용하면 문제가 생기는 이유**

앱을 실행하면 main 스레드에서 작동하는 걸 확인할 수 있다.

<img width="392" alt="image" src="https://user-images.githubusercontent.com/48466830/89296481-5c00fe80-d69d-11ea-8944-9d22a340409a.png">





작업이 큐에 들어가면서,  큐는 block상태가 되고, 작업은 큐가 끝날 때까지 기다리게 된다. (DeadLock) 

<img width="740" alt="image" src="https://user-images.githubusercontent.com/48466830/89296646-94a0d800-d69d-11ea-9dae-a81c3237b809.png">





다시 말해서 serial.sync를 호출하면 항상 deadlock이 발생하는 게 아닌, 같은 큐를 sync로 호출할 경우에 발생한다.

다른 serial queue를 호출할 때는 정상적으로 작동한다.



## Qos (Quiality of service)

background 작업들을 시스템이 실행하는 최선의 방법을 결정하기 위한 정보를 제공한다.

* userInteractive: 가장 높은 우선순위, UI 갱신, 애니메이션 처리
* userInitiated: 그 다음 우선순위, 유저가 앱에 기대하는 핵심 정보를 로딩할 때 사용한다.
* default: 앱 초기화 혹은 유저를 대신해 수행하는 작업
* utility: 바로 완료되지 않아도 앱사용에 지장이 없는 부가적인 작업
* background: 앱이 background로 갔을 때 할 작업