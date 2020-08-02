# Generic

---

Generic code 는 프로그래머가 정의한 필요조건에 따라 어떤 타입에서도 작동 가능한 유연하면서도 재사용 가능한 함수와 타입을 작성할 수 있게 해줍니다. 

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



