# Transition Style, Presentation Style

---

## Modal Transition Style

### Cover Vertical

기본적인 modal 방식이다. 아래에서 위로 올라온다.



### File Horizental 

가로 방향으로 가운데를 축으로 카드가 뒤집히듯이 돈다.



### Cross Dissolve

화면 교차. 처음 화면이 흐려지고 전환 될 화면이 강해지는? 효과이다.



### Partial Curl

종이 넘기는 효과



## Modal Presentation Style

화면 크기에 영향을 받는다. Regular, Compact가 존재한다. 아이폰의 가로는 Compact, 세로는 Regular 아이패드는 가로, 세로 Regular이다.



### Full Screen

화면 전체가 뒤덮게 띄운다.



### PageSheet

가로가 Regular일 경우, 가로는 세로와 동일하며, 세로는 기기의 세로 길이이다.

가로가 Compact이면 FullScreen과 동일하게 작동한다.



### FormSheet

가로가 Regular일 경우, content 영역의 크기를 그대로 유지하고, dimmedView 처리도 해준다.

기기가 가로 모드일 경우 키보드가 올라 왔을 때 뷰가 올라가는 처리를 자동으로 해준다.



### CurrentContext

view controller를 시작으로 해서 definesPresentationContext가 true 설정되어 있는 viewController의 영역을 새로운 뷰로 띄워준다. splitView에서 root view controller를 바꿀때 유용하게 사용될 수 있다.



### OverFullScreen

FullScreen과 동일하지만 뷰가 투명할 경우 아래 쌓인 뷰가 보인다. FullScreen에서 아래 깔리는 뷰는 뷰 계층에서 지워지지만 over를 적용하면 사라지지 않는다.



### OverCurrentContext

Current Context와 동일하지만 아래 쌓인 뷰가 보인다. 



### Popover

팝업창처럼 뷰를 띄워준다. 가로가 Regular일 경우에만 적용되며 가로가 Compact일 경우 기본적으로 FullScreen이 적용된다.

* preferredContentSize 속성을 오버라이드 해줘야한다.
* popoverPresentationController 속성을 통해 어떤 뷰를 중심으로 띄울 것인지 설정해줘야한다. barButtonItem 속성을 설정하거나, sourceView와 sourceRect 속성을 설정해준다.

