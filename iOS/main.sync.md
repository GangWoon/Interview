# main.sync

---

Dispatch queue에서 왜 main.sync를 부르면 dead lock이 발생한다.

Main queue에 넣은 task가 완료되기 전까지 메인 스레드는 blocking waiting 상태가 된다. 하지만 이 task는 메인 스레드에서 serial 하게 실행이 되는 main queue에 들어가 있다. 따라서 block 상태로 빠진 메인 스레드는 작업이 영영 시작되지 않는다. 

