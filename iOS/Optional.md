# Optional

---

``` swift
public enum Optional<Wrapped>: ExpressibleByNilLiteral {

  case none
  case some(Wrapped)
  
  public init(_ some: Wrapped) { self = .some(some) }
  
  public init(nilLiteral: ()) { self = .none }
  
  public var unsafelyUnwrapped: Wrappped {
    get {
      if let x = self {
        return x
      }
       _debugPreconditionFailure("unsafelyUnwrapped of nil optional")
    }
  }
}
```

Optional은 지네릭으로 구현되어 있으며, Optional로 래핑된 값이 있거나, nil을 반환한다. 





## Optional  UnWrapping

---

### if let 

``` swift
if someBoolean == true {
  
}
```

if 문을 만족하면 안에 내용을 실행하고 그 이후 작업을 실행한다. 만족하지 못했을 경우 바로 if문 이후의 작업을 계속 실행한다.



### guard 문 (빠른 탈출)

``` swi
guard someBoolean == true else { return ... }
...
```

guard문을 만족하면 아래 작업을 실행하고 만족하지 못할 경우에 return이나 break, continue 등 해당 루프에서 벗어나도록 한다.





