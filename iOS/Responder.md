# Responder

---

앱은 UIResponder 타입의 객체를 이용해서 여러 이벤트를 처리한다.

Responder가 이벤트를 받으면 이를 반드시 처리하거나, 다른 Responder가 처리할 수 있도록 넘겨야 할 의무가 생긴다. 

위에서 언급했듯이 First Responder가 이벤트를 처리하지 못한다면 처리할 수 있는 Responder가 나올 때 까지 responder chain이 발생한다. 

![f17df5bc-d80b-4e17-81cf-4277b1e0f6e4](https://user-images.githubusercontent.com/48466830/88263279-bf397b00-cd04-11ea-9602-70cf9a3765c9.png)

UIView: 자신이 최상위 뷰일 경우에 다음 Responder를 해당 view controller로 넘긴다.

UIViewController 

	* 최상위 view controller라면 다음 Responder를 window로 넘긴다.
	* 다른 view controller에 의해 띄워진 view controller일 경우 해당 view controller를 띄워준 view controller로 넘긴다.

UIWindow:  다음 Responder를 UIApplication으로 넘긴다. (AppDelegate 객체가 UIResponder 타입이고, view, view controller, UIApplication이면 안된다.)

AppDelegate: 처리할 수 없다면 nil을 반환한다.



## Determining Which Responder Contained a Touch Event

UIKit은 뷰를 기반으로 hit testing을 진행해서 이벤트가 발생한 위치를 확인한다.UIView의 hitTest(_: with:) 메소드는 뷰 계층을 탐색하면서 해당 터치 이벤틀 가지고 있는 가장 깊은 계층의 뷰를 찾아 내서 반환한다. 이 뷰가 해당 터치 이벤트의 First Responder가 된다.

UITextField, UITextView, UISearchBar는 다른 뷰와 다르게 canBecomeFirstResponder가 true이다. 추가적으로 편집모드가 있어서 시스템 키보드가 등장하면 notification을 전달한다. 이후 FirstResponder 권한이 사라지면 키보드가 내려간다. 