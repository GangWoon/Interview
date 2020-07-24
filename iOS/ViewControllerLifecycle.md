# View Controller Life Cycle

---

### init(nibName: bundle:)

nib파일을 통해 view controller을 초기화 한다. view를 만들 때 사용할 정보를 view controller에 저장한다.



### init?(coder:) 

스토리 보드로 view controller를 초기화 한다. 실패 가능 초기화



### loadView()

view를 실제로 생성해서 메모리에 로드한다. view controller의 view를 이 시점에서 초기화한다.

super.loadView를 호출하면 안된다.



### viewDidLoad

view가 메모리에 로드된 직후에 호출된다. loadView이후에 view, view controller에 추가적인 설정을 한다.



### viewWillAppear

view가 view hierarchy에 추가되기 직전, view 나타나는 애니메이션이 설정되기 전에 호출된다. view가 화면에 나타나기 전에 필요한 추가 작업을 수행한다.



### viewDidAppear

view가 view hierarchy에 추가된 직후, view에 추가적인 설정을 해 줄 수 있다.



### viewWillDisappear

view가  view hierarchy에 제거되기 직전, view가 사라지는 애니메이션이 설정되기 전에 호출된다. view의 변화를 저장하거나, first responder를 내려놓는 작업을 한다.



### viewDidDisappear

view가  view hierarchy에 제거된 후 호출 된다. view가 사라질 때 필요한 추가작업을 수행한다.



### didReceiveMemoryWarning

시스템에서 메모리가 부족하면 메세지를 보내는 데 바로 이 메소드이다. 메모리를 확보를 수행한다.















