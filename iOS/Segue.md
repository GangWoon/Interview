# Segue

---

## show (push)

show(: sender:) 메소드이다. navigation controller는 새로운 view controller를 스텍에 푸시한다. 해당 메소드는 view controller 계층을 확인하면서(navigation, split view controller가 있는지) show(: sender:)를 오버라이드한 view controller를 찾아서 사용하고 없는 경우 nil을 반환하고 present(_:, animated:)를 사용한다.



show(:sender:)를 오버라이드 할 때 super를 부르면 안된다.

show 내부에서 targetViewController(forAction: sender:)을 호출하고, 이 과정에서 루프가 발생한다.



## showDetail(Replace)

showDetailViewController(_: sender:) 메소드이다. UISplitViewController에서만 사용 가능하고, split view의 detail controller를 바꿔준다. 다른 view controller에서 사용하면 present(:, animated:)로 동작한다.



## Present Modally

presentation style, transition style에 따라 modal. style에 맞는 뷰컨트롤를 발견할 때 실제로 present한다.



## Present as Popover

가로 길이가 보통 사이즈인 경우에 popover로 띄우지만, 가로 길이가 compact 사이즈이면 전체화면 modal과 같다.