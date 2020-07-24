# Operation Queue

---

Concurrent DispatchQueue에 대응하는 Cocoa 프레임워크의 객체이다. 작업의 실행순서에 다른 요소를 고려한다. 가장 중요한 요소는 주어진 작업이 다른 작업이 끝나는 것에 의존하는 지의 여부이다. 의존성을 설정하여 복잡한 실행 순서 그래프를 구성할 수도 있다.

OperationQueue에 들어가는 작업은 반드시 Operation객체의 인스턴스여야만 한다. 이는 수행하고자 하는 작업과 데이터를 캡슐화한 객체이다. Operation은 추상 클래스이기 때문에, Operation 타입의 구체 클래스를 사용하거나, 직접 상속받은 클래스를 구현해야한다. KVO을 위한 프로퍼티를 제공해서, 이를 통한 작업의 진행상황을 모니터링 할 수 있다. OperationQueue 자체는 여러 작업을 동시에 수행하지만, 의존성을 설정하는 것으로 순차적으로 실행하게 만들 수 있다. operation을 중간에 멈출 수 있다.

operation을 보통 queue에 넣어서 사용하지만 operation 객체의 start() 메소드를 사용해서 수동으로 실행시킬 수 있다. 하지만 동시에 수행되는 여부는 알 수 없으며, operation queue를 사용하면 처리 해준다.