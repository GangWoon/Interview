# Notification Center

---

Notifiaction은 앱 상에서 발생하는 이벤트를 캡슐화(encapsulate) 한것이다.

Notification Center는 Notification을 받고자 하는 객체를 등록하고 Center에서 Notification이 발생하면 등록된 객체에게 broadcast하는 역활이다.

NotificationQueue를 통해서 Notification을 일정시간 지연시키거나, 비슷한 Notifiaction을 묶어서 한번에 전송할 수 도 있다. 비동기적 이벤트를 처리할 수 있다.

removeObserver는 iOS 9.0 이상 버전에서는 시스템에서 해제해준다.



```swift
.post(name: NSNotification.Name,

​         object: Any?,

​         userInfo: [AnyHashable : Any]?)
```

post 메소드를 통해 notification center로 보내면 center에서는 등록되어 있는 리스트를 확인하고 해당 이벤트를 구독한 객체에게 notification을 보낸다. 동기적 메소드이다.



```swift
            .addObserver(observer: Any>,
                         selector: Selector,
                         name: NSNotification.Name,
                         object: Any?)
```

addObserver 메소드를 통해서 center에 받고 싶어하는 Notification을 등록합니다.



```swift
.enqueue(notification: Notification,
                 postingStyle: NotificationQueue.PostingStyle,
                 coalesceMask: NotificationQueue.NotificationCoalescing,
                 forModes: [RunLoop.Mode]?)
```

coalescing이란 notification을 합쳐서 한꺼번에 처리하는 기능이다.

NotificationQueue.PostingStyle enum case는

* asap: 현재 스레드의 RunLoop가 끝나는 대로 post한다.
* whenIdle: 현재 스레드가 idle 상태가 되면 post한다.
* Now: coalescing을 거친 후 즉시 post된다. (동기적)

NotificationQueue.NotificationCoalescing enum case는

* none: 해당 notification에 대해서는 coalescing이 일어나지 않는다.
* onName: 이름이 같은 notification에 대해서 coalescing한다.
* onSender: 같은 객체가 보낸 notification에 대해서 coalescing을 한다.

RunLoop.Mode는 현재 스레드의 RunLoop가 Notification을 post할 수 있는 상태를 제한한다.

