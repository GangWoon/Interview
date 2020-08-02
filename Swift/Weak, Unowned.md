# Weak, Unowned

---

weak와 unowned의 가장 큰 차이점은 zeroing이다.

weak는 zeroing을 지원합니다. weak reference를 하면 참조 객체가 메모리에서 해제되면 포인터를 그대로 가리키는 게 아닌 nil이 할당된다.

반면에 unowned는 zeroing을 지원하지 않습니다. 

메모리 주소를 그대로 가르키지만 해당 메모리에는 인스턴스가 없기 때문에 fatal error가 발생한다.