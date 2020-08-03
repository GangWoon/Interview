## **Stored Properties**

인스턴스의 부분으로 저장된다.

클래스와 구조체에서만 사용한다.

var가 붙으면 변수이며, let이 붙으면 상수이다.




#### 상수 구조체 인스턴스의 stored properties

```
let someStruct = struct(...)
someStruct.value = 4 // value는 변수, 에러가 발생한다.
```

인스턴스를 할당할 때 상수로 선언하면, 프로퍼티가 변수여도 수정할 수 없다.

이는 구조체가 값 타입이기 때문에 인스턴스가 상수로 선언되면 모든 프로퍼티가 상수로 표시된다.




#### Lazy Stored Properties

lazy 변수는 처음 사용되기 전까지는 할당되지 않으며, 호출을 받으면 메모리에 올라간다.

초기 값이 존재하지 않고, 이후에 값이 생성되기 때문에 let으로 선언할 수 없다.






## **Computed Properties**

실제 값을 저장하는 프로퍼티가 아닌, 특정 상태에 따른 값을 연산하는 프로퍼티이다.

필수적인 get과 선택적인 set으로 구현가능하다. (setter를 사용하려면 계산된 값을 저장할 저장 프로퍼티가 필요하다.)

해당 프로퍼티에 값을 할당하는 형식이 아니기 때문에 let을 사용할 수 없고 var만 가능하다.

저장 프로퍼티와 다르게 명시적으로 타입을 선언해야한다.






## **Property Observer**

프로퍼티 값 변경을 감시하고 대응한다.

모든 저장 프로퍼티를 감시하도록 정의할 수 있지만, 지연 저장 프로퍼티는 예외이다. 

willSet(newValue), DidSet(oldValue)이 존재한다.






## **Global and Local Varialbes**

전역 변수는 함수나 메소드, 클로저 또는 타입 컨텍스트 밖에서 정의된 변수이며, 지역 변수는 함수, 메로스 클로저 또는 타입 컨텍스트 안에서 정의된 변수이다.

전연 변수는 언제나 지연 계산되며, lazy를 명시할 필요가 없다.






## Type Properties

인스턴스 프로퍼티는 특정 타입의 인스턴스에 속한 속성이다. 

타입 프로퍼티는 타입에 속한 프로퍼티를 말한다. 

값 타입을 위해 저장 프로퍼티와 계산 프로퍼티를 정의할 수 있다. (Static)

클래스를 위한 계산 타입 프로퍼티만 정의할 수 있다. (Class) 

```
class Dog {
    static let age: Int = 3
    class var info: String {
        return "강아지는 \(age)살"
    }
}

class GangWoon: Dog {
    override class var info: String {
        return "강운이는 \(age)살"
    }
}
```