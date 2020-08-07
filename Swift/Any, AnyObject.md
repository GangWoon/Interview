- Any can represent an instance if any type at all, including function types and optional types.
- AnyObject can represent an instance of any class type.

Any는 모든 타입을 말하며, Anyobject는 클래스 타입을 말한다.

AnyObject는 모든 클래스가 암시적으로 준수하는 프로토콜이다.





------





NSCache를 사용하면서 들었던 의문점이 생겼다.

UIImage는 별다른 변환 없이 사용 가능했고(UIImage는 클래스이기 때문에), **URL은 AnyObject로 변환해서 집어 넣줘야 했다.**

AnyObject는 클래스가 암시적으로 준수하는 프로토콜이라고 정의했는데, 어떻게 URL(Struct)이 변환이 되는 건지 이해할 수 없었다.

```
let cache = NSCache<AnyObject, AnyObject>()

func insertImage(_ image: UIImage, for key: URL) {
    cache
        .setObject(image,
                   forKey: key as AnyObject)
}
```









코드를 보면 미세한 차이가 있는데, 바로 "GangWoon"과 GangWoon의 차이다.

```
let anyArray: [Any] = [1, 2, 3, "GangWoon"] // [1, 2, 3, "GangWoon"]
let anyObjectArray: [AnyObject] = [1 as AnyObject,
                                   2 as AnyObject,
                                   3 as AnyObject,
                                   "GangWoon" as AnyObject] // [1, 2, 3, GangWoon]
```







둘의 차이점이 있는지 확인해봤지만, 전부 String 타입이였다.

```
for element in anyObjectArray {
    if element is Int {
        print("\(element) is Int")
    } else if element is String {
        print("\(element) is String")
    }
}                  
```

![image](https://user-images.githubusercontent.com/48466830/89643111-0bcdaa80-d8f0-11ea-8603-574d7abec3fe.png)







Int를 NSNumber로 String을 NSString으로 변경했을때도 같은 결과가 나왔다.

```
for element in anyObjectArray {
    if element is NSNumber {
        print("\(element) is NSNumber")
    } else if element is NSString {
        print("\(element) is NSString")
    }
}
```

![image](https://user-images.githubusercontent.com/48466830/89645036-f5295280-d8f3-11ea-8956-4deb8c122c8b.png)







String과 NSString의 차이점을 확인해봤다.

아래와 같이 String은 ""안에 문자열을 표시했고, NSString은 ""이 없었다.

```
let stringArray: [String] = ["Swift", "String"] // ["Swift", "String"]
let nsStringArray: [NSString] = ["Objc", "NSString"] // [Objc, NSString]
```







NS는 내부적으로 NSObject를 상속받기 때문에 String이 NSString으로 브릿징되면서 class로 변경되고, AnyObject로 변경 가능한 것이였다. 이런 과정을 브릿징이라고 표현했고, swift와 objc를 사용할 때 시스템에서 자동적으로 변경해준다.



> In Objective-C, the id type represents objects that are instances of any Objective-C class. The id type is instead imported 
> by Swift as the Any type. When you pass a Swift instance to an Objective-C API, it's bridged as an id parameter so that it's usable in the API as an Objective-C object. When id values are imported into Swift as Any, the runtime automatically handles bridging back to either class references or value types.









#### 참고자료

[medium.com/@mimicatcodes/any-vs-anyobject-in-swift-3-b1a8d3a02e00](https://medium.com/@mimicatcodes/any-vs-anyobject-in-swift-3-b1a8d3a02e00)