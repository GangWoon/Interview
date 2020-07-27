# CALayer

---

이미지 기반 컨텐츠를 관리하고 해당 컨텐츠에서 애니메이션을 수행 할 수 있는 객체입니다. UIView는 레이아웃, 터치 이벤트 등 많은 작업을 처리한다. view 위에 컨텐츠나 애니메이션을 그리는 행위는 직접적으로 다루지 않고 UIKit이 Core Animation으로 delegate한다. 즉 view 위에 contents를 표시하는 건 CALayer가 담당한다. UIView는 사실 CALayer를 감싸고 있고, UIView bounds가 변경되면 자신의 root layer bounds가 변경된다. root layer는 자동적으로 UIView에 맞춰서 변경된다.