# Generic

---

generic 코드는 유연하게 작성할 수 있고, 재사용가능한 함수와 타입이 어떤 타입과 작업할 수 있도 요구사항을 정의한다.

중복을 피하고 명확하게 표현하고, 추상적인 방법으로 코드를 작성할 수 있다.

< T > 를 타입 인자라고 한다.

< T: SomeProtocol > , < T > where: SomeProtocol 로 제약을 줄수 있다.

Protocol를 정의할 때, 연관 타입을 사용할 수 있다.

``` swift
protocol SomeProtocol {
	typealias someType
}

class SomeThing: SomeProtocol {
	typealias someType = Int
}
```



