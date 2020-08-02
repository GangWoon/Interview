# Managing App's Life Cycle

* Not Running: App이 아직 실행되지 않았거나 시스템이 완전히 종료된 상태
* Inactive: Launch screen ,App이 실행중이지만 시스템 알림(통화)이 화면을 덮어서 이벤트를 받지 못하는 상태 
* Active: App이 실행중이고 이벤트를 받고 있는 상태
* Background: App이 다른 앱으로 전환되거나 홈 화면으로 나갔을 때 실질적으로 동작을 하고 있는 상태(음악)
* Suspended: 앱을 다시 실행할 경우 빠른 실행을 위해 메모리에만 올라가 있는 상태(메모리가 부족하면 release시켜서 메모리를 확보한다.)

# Scene Based Life Cycle

Later...



---

# App Based Life Cycle

iOS 12 이전, scenes를 지원하지 않는 경우 UIKit은 UIApplicationDelegate 객체로 모든 life cycle을 보낸다.(기본적으론 SceneDelegate로 보낸다.) 모든 앱의 windows를 관리한다.

앱이 실행된 후 inactive 또는 background 상태가 된다.

![4d403429-fa30-4706-863f-5e3617ee21d0](https://user-images.githubusercontent.com/48466830/88169584-27318800-cc57-11ea-80af-d7b7835bff34.png)

* 앱이 실행될때 데이터 구조와 UI를 초기화 시킨다.

  ### Responding to the Launch of Your App

  홈 화면에서 앱 아이콘을 누르면 시스템이 앱을 시작한다. 앱이 특정 이벤트를 요청한 경우 시스템은 백그라운드에서 앱을 시작하여 해당 이벤트를 처리 할 수도 있다.

  프로세스에서 발생하는 중요한 이벤트에 응답하는 객체(UIApplicationDelegate protocol을 준수하는 객체)가 있다. Secne Based App도 AppDelegate를 사용해서 시작 및 종료 이벤트를 관리한다. 시작시 UIKit은 UIApplication 객체와 Appdelegate를 자동적으로 만들고, 앱의 기본 이벤트 루프를 시작한다.

  * Initialize Your App's Data Structures

    application(_: willFinisLaunchingWithOptions: )

    application(_: didFinishLaunchingWithOptions: )

    앱 데이터 구조를 초기화한다.

    앱에 실행에 필요한 자원이 있는지 확인한다.

    ex) 사용자가 앱에 마지막으로 수행 한 작업을 반영하기 위해서 didFinishLaunchingWithOptions 내부에서 처리한다.

  * Move Long Running Tasks Off the Main Thread

    application(_: didFinishLaunchingWithOptions: )이 끝나야지 앱의 인터페이스를 보여줄 수 있다. 그러므로 많은 작업을 처리하지 안된다.

  * Determine Why Your App Launched

    

* 활성화시 UI 구성을 완료하고 사용자와 상호 작용 준비를 한다. 

  ### Preparing Your UI to Run in the Foreground

  Foreground로 전환은 사용자 작업에 대한 응답이다. Foreground 전환을 사용하여 앱의 UI를 업데이트하고 리소스를 확보하며 사용자 요청을 처리하는 데 필요한 서비스를 시작합니다.

  * Update Your App's Data Model when Enterung the Foreground

    앱을 foreground로 전환하기 전에 inactive 상태에서 앱을 시작합니다. 백그라운드에있는 앱의 경우 applicationWillEnterForeground(_:)을 호출해서 inactive state로 이동한다. 이 시점에서 디스크에 리스소를 로드하고 네트워크에서 데이터를 가져온다.

  * Configure Your User Interface and Initial Taks at Activation

    시스템은 UI가 보여지기 직전에 active state로 전환시킨다. 보여질 viewcontroller를 바꾸거나 뷰와 컨트롤러의 데이터를 업데이트 시키기 좋은 시점이다. applicationDidBecomeActive(_: ) 메소드 내부에서 구현하면 된다.

    active는 UI를 사용자에게 표시하기 전에 UI를 마무리하는 시간이다. active를 차단할 수 있는 코드는 실행하지말고 필요한 모든 것을 준비하자. 데이터가 앱 외부에서 자주 변경되면 foreground로 변경하기 전에 background 작업을 사용해서 네트워크에서 데이터를 받아 온다.

  * Start UI Specific Tasks When Your View Appears

    active 메소드가 반환되면 UIKit은 windows를 보여준다. 관련 viewcontroller에 해당 view가 표시 될 것임을 알린다. 애니메이션을 실행시키거나 미디어를 실행시키 적당한 시점이다.

* deactivation되면 데이터를 저장하고 앱의 동작을 정지시킨다.

  ### Preparing Your UI to Run in the Background

  유저가 foreground 앱을 종료시키면 앱이 suspend되기전에 background state로 이동한다. 시스템은 직접 background state 오거나 suspend 되기전에 잠시 background로 와도 중요한 작업을 수행할 시간을 준다.

  앱이 background에 있으면 가능한 적은양의 작업을 실행해야하고, 작업을 중지하고 자원을 해제 해야한다. 

  * Quiet Your App upon Deactivation

    시스템 알림(전화가 왔을 때)을 표시하기 위해서 일시적으로 중단해야 하는 경우에도 시스템이 앱을 deactivate시킨다. 시스템 알림의 경우 사용자가 알림을 닫으면 시스템이 앱을 다시 reactivates시킨다. 비활성화 되는 동안에 UIKit은 applicationWillResignActive(_ :)를 호출한다. dispatch, operation 큐를 중지시키고 사용자 정보를 디스크에 저장한다.

  * Release Resources upon Entering the Background

    앱이 background로 전환되면 메모리를 해제하고 앱이 보유하고 있던 자원을 release 시킨다. background로 들어가면 applicationDidEnterBackground(_: )메소드가 호출된다. 이미지 파일을 직접 읽은 것들은 버리고, 공유 하드웨어(카메라) 자원을 release시킨다.

    NSCache 객체를 사용하여 관리하는 객체는 해제 할 필요가 없다. 시스템이 자동으로 처리해준다.

  * Prepare Your UI for the App Snapshot

    앱이 background로 진입해서 delegate method가 반환되면 Uikit은 현재 사용자 인터페이스의 스냅 샷을 만든다. 앱을 다시 foreground로 가져올 때 이미지를 일시적으로 표시한다.

* Background 상태가 되면 중요한 작업을 완료하고 가능한 많은 메모리를 해제해야한다.
* 종료시 모든 작업을 중지하고 공유 자원을 release시켜야 한다. applicationWillTerminate(_: )

