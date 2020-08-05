# Memory Management

클래스 인스턴스, 참조 타입의 값들은 head 영역에 저장된다.
저장되는 값을 명시적으로 삭제하지 않으면, 메모리에 계속 남게 된다.
이런 상황을 메모리 릭이라고 부른다.
이를 해결하고자 애플은 MRC, ARC를 제공한다.



### **RC (Reference Counting)**

컴파일 타임에 참조에 따라서 카운트를 관리하며, 이를 사용해 메모리 관리를 처리한다.





### **MRC (Manual Reference Counting)** 

objc는 rc방법을 프로그래머가 직접 작성해서 처리했다.
retain은 rc를 증가시킬 때 사용하며, release는 감소 시킬때 사용한다.

![0*uPhaQKYGc7AAdTvw](https://user-images.githubusercontent.com/48466830/89394034-865ac680-d746-11ea-87cc-80dd42b54e5d.jpg)





### **ARC (Auto Reference Counting)**

컴파일 타임에 자동으로 실행되며, 정해진 규칙에 따라서 rc를 관리해준다.

정해진 규칙에는 세 가지가 존재한다. strong, weak, unowned.



#### **strong Reference** 

참조를 할 때 rc를 증가시키며, 서로 강하게 참조할 경우 rc가 0이 아닐 경우가 생긴다. (retain cycle)





#### **weak**

참조를 할 때 rc를 증가시키지 않는다.
인스턴스가 메모리에서 해제되면, 자동으로 nil이 할당되며(Zeroing), 메모리를 반환한다.
그러므로 weak, var, ?은 세트이다.





#### **unowned**

weak와 유사하지만 가장 큰 차이점은 zeroing을 지원하지 않아서 위험하다.
인스턴스가 메모리에서 해제되면, 메모리에 쓰레기 값이 그대로 존재해서 접근할 경우 앱이 깨지는 현상이 발생한다.