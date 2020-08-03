# class, struct

### 공통점

* 프로퍼티, 메소드를 갖는 컨테이너를 말한다.
* subscript 구문을 사용하여 값을 접근할 수 있는 subscript를 정의한다.
* extension을 사용할 수 있다.
* protocol을 채택할 수 있다.



### Class

* 상속을 할수 있다.
* deinit을 한다. arc가 다룬며, 해지할 때 필요한 작업을 수행한다. deinit은 자동으로 호출되며 수동으로 호출할 수 없다. 서브 클래스에 deinit이 제공되지 않더라도 슈퍼 클래스의 deinit가 항상 호출 된다.
* reference type이다.



### Value type vs Reference type

Value type: 값 복사를 할 경우에 인스턴스를 복사하면 각 인스턴스는 유니크한 데이터를 갖는다. 하나의 인스턴스를 변경하면 다른 인스턴스는 변경되지 않는다.

Reference type: 참조 복사를 할 경우에 두 인스턴스는 데이터를 공유한다. 즉 데이터를 복사한 게 아니라 참조를 복사한다.



class 내부에서 구조체를 사용하면 별다른 문제가 없지만, struct 내부에 공유 클래스 인스턴스를 사용한다면 조심해야한다.





