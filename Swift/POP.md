# POP

시대의 흐름은 OOP(Object Orient Programming)에서 POP(Protocol Orientd Programming)로 넘어간다. 



### 왜 흐름이 바뀌었는가? 

OOP는 클래스를 기반으로 상속을 통한 유연함을 유지했다. 하지만 상속은 생각보다 유연하지 못했다.

추상화된 클래스를 구체화했을 때 공통적인 책임을 갖는 여러 클래스가 존재할 수 있고 중복된 코드가 생성된다.

참조 타입을 사용하면서 발생하는 문제들(멀티 스레드 환경에서의 다중 접근, 동적 할당과 참조 카운팅으로 인한 자원 소모)이 있다.



위와 같은 문제점을 해결하고자 Swift 2.0에서는 **Protocol + Extension + Generic** 조합을 발표한다.

### class만 가능했던 상속 기능을 Protocol + Extension을 사용해서 struct에서도 가능하게 만들었다.

``` swift
protocol Dog { }
extension Dog {
    func run() { .. Running .. }
}
struct Jindo: Dog { }
let jindo = Jindo()
jindo.run()
```





### class는 단일 상속만 지원하지만 protocol을 채택하면 다중 상속처럼 구현할 수 있다.

```swift
protocol Sittable { }
extension Sittable {
    func sit() { }
}
struct GangWoon: Dog, Sittable { }
let gangwoon = GangWoon()
gangwoon.run()
gangwoon.sit()
```





### Generic과 같이 사용하면서 더 추상적일 수 있다.

```swift
protocol HeaderViewProtocol {
    associatedtype Content
    func setHeader(_ content: Content)
}

extension UIImageView: HeaderViewProtocol {
    func setHeader(_ content: UIImage) { image = content }
}

extension UILabel: HeaderViewProtocol {
    func setHeader(_ content: String) { text = content }
}
```





## Protocol을 사용하면서 실수 했던 점

``` swift
return IssueTrackerNetworkImpl.shared
			...
```

IssueTrackerNetwork라는 프로토콜을 구체화한 IssueTrackerNetworkImpl이란 구조체가 있다. 
그리고 위 코드는 IssueTrackerNetworkImpl을 바로 사용했다. 
Protocol을 사용하는 근본적인 이유 추상화된 개념을 사용해서 유연성을 늘리고 싶어서 사용했지만, 정작 구체 타입을 사용해서 코드를 구현했다. 프로토콜을 사용했지만 유연하지 못한 코드이다.





# Reference

https://o-o-wl.tistory.com/13

https://academy.realm.io/kr/posts/protocol-oriented-programming-in-swift/

