# Swift 성능 이해



Swift에서 성능에 영향을 미치는 세 가지 요인이 있다.

Allocation: 인스턴스화를 통해 Stack , Heap에 저장할지

Reference Counting : 인스턴스를 통해 RC가 얼마나 발생하는지

Method Dispatch: 인스턴스 메소드를 사용했을 때 Method Dispatch가 정적인지 동적인지





## Allocation

Swift는 메모리 할당 및 해제를 담당한다.

### Stack

Stack은 LIFO(Last In First Out)구조로 메모리 할당및 해제가 편하다.(시간 복잡도는 O(1))

Stack은 Stack Pointer를 사용해서 할당및 해제를 처리한다.

Heap보다 속도가 빠르다.



### Heap

Stack과 달리 dynamic한 할당 방법을 사용한다.

Heap 영역에서 사용하지 않는 블록을 찾아서 메모리 할당을 한다. 해제하기 위해서 해당 메모리를 적절한 위치로 다시 삽입한다.

무결성 보호를 위해 세마포어등 여러가지 방법이 있다.



## Value and Reference Semantics

메모리 할당 시 Stack, Heap의 기준은 무엇인가?

Semantics로 결정되고, Value Semantics 와 Reference Semantics로 나뉜다.



### Value Semantics

타입들의 인스턴스를 Stack에 할당한다. 

struct, enum, tuple 그리고 기본 타입들은 Value Semantics를 따르고 있다.

내부 인스턴스의 개수의 따라서 2words size로 Stack에 할당된다.

struct는 인스턴스를 생성하여 다른 인스턴에 할당 할 때, 전체 값이 그대로 복사 된다. Heap 영역을 사용하지 않기 때문에 RC가 발생하지 않는다.



### Reference Semantics

Stack에는 Reference의 주소를 할당하고, 실질적인 데이터는 Heap에 할당한다.(stack에 할당되는 Value Semanctics 보다 더 큰 비용이 발생)

class, function이 Reference Semantics이다.

class는 Heap에 4words size가 할당된다.

class는 Reference Semantics로 항상 하나의 identity이다.



## 성능 올리기

String은 구조체이지만 Character 크기에 따라서 전체 크기가 다를 수 있기 때문에 Value Semantics가 아닌 Reference Semantics를 사용하게 된다. (추론은 정확하지 않지만 결과는 정확하다.)



### Reference Counting in Struct

struct 내부 프로퍼티로 Reference Semantics가 존재하면 RC가 발생한다.

stack에서 하나 이상의 Reference Semantics 내부 프로퍼티가 존재한다면 class를 사용하는 것보다 더 큰 오버헤드가 발생한다.



## Method Dispatch

프로그램이 메소드를 호출하는 과정을 말한다. 

결정되는 시점에 따라서 Static Method Dispatch, Dynamic Method Dispatch로 나뉜다.



### Static Method Dispatch

컴파일 시점에 컴파일러가 메소드의 코드 위치를 파악할 수 있어서, 런타임 시점에 찾는 과정이 불필요하다.

메소드 인라이닝을 통한 코드 최적화를 시행할 수 있다.

* 메소드 인라이닝: 메소드를 호출할 때 메소드를 호출하지 않고 결과값을 즉시 반환해서 성능을 개선한다.



### Dynamic Method Dispatch

컴파일 시점에 컴파일러가 메소드의 코드 위치를 파악할 수 없어, table에 구현을 참조해 해당 메소드에 대한 정보를 가져와 코드를 실행한다.

최적화 작업을 할 수 없기에 Static Method Dispatch 보단 성능이 감소된다.



### 왜 메소드의 위치를 파악할 수 없는가?

답은 다형성에 있다.

컴파일 타임에 명확하게 self가 누구인지 알 수 없을 것이다. 

(아래 사진에서 포문 안에 있는 d가 컴파일 시점에 Point인지 Line인지 알 수 없을것이다.)

<img width="1030" alt="3_2_polymorphism_dynamic_dispatch_1" src="https://user-images.githubusercontent.com/48466830/94152228-4c8c7d80-feb6-11ea-9957-8cb977873455.png">





이를 해결하기 위해 컴파일 타임에 컴파일러는 클래스에 필드 하나를 추가한다.

타입 정보에 대한 포인터이며, 타입 정보는 static memory에 저장된다.

<img width="1029" alt="3_2_polymorphism_dynamic_dispatch_3" src="https://user-images.githubusercontent.com/48466830/94152237-4f876e00-feb6-11ea-97e9-2422060276ec.png">





해당 포인터를 통해서 타입정보를 확인하고 메소드를 호출한다.

해당 인스턴스를 암묵적으로 파라미터로 보낸다.

```swift
override func draw(_ self: Line) { ... }
```

<img width="1029" alt="3_2_polymorphism_dynamic_dispatch_5" src="https://user-images.githubusercontent.com/48466830/94152248-51e9c800-feb6-11ea-9ed3-9a1a9071a58c.png">





## Reference 

https://corykim0829.github.io/swift/Understanding-Swift-Performance/#