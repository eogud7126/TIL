## UIKit2

## 1. 뷰 컨트롤러 계층

- 모든 앱은 하나 이상의 뷰 컨트롤러를 가진다
- 사용자 인터페이스 뿐만 아니라 다른 화면 전환
- 사용자와 화면간의 인터렉션
- Root View Controller
  - window의 컨텐츠 뷰
  - window는 오직 하나의 루트 뷰 컨트롤러만 가질 수 있음
  - 루트뷰 컨트롤러의 뷰가 윈도우의 서브뷰로 추가되며 윈도우를 채우도록 크기고정
  - ***responder** **chain**: 발생한 이벤트를 어떻게 처리할지에 대한 클래스!

## 2. Container View Controller

1. UISplitViewController
   - Master-Detail 구조의 인터페이스, 화면을 두개의 뷰 컨트롤러의 컨텐츠를 동시 혹은 단일로 표시
   - Parent-Child 계층구조, 마스터 뷰컨트롤러 영역에서는 Detail View Controller에 표시될 내용을 결정
2. UIPageViewController
   - 사용자 제스쳐 또는 코드로 한 화면 안에서 두 개 이상의 뷰 컨트롤러의 컨텐츠를 동시에 표시
   - 여러개의 뷰 컨트롤러를 child로 가지며 scroll 또는 Page Curl 트랜지션 애니메이션을 지원
   - Parent - Child 계층 구조



## 3. UINavigationController

- 계층적 컨텐츠를 탐색하기 위한 스택 기반의 Container View Controller
- Left Item
  - navigationItem.leftBarButtonItem으로 접근 가능
  - 네비게이션 컨트롤러 루트 뷰 컨트롤러가 아닌 모든 Child 뷰컨트롤러들은 이전 화면으로 돌아갈 수 있는 Back버튼 제공
  - Back버튼 커스텀 backBarButtonItem으로 설정할 수도 있고 설정하지 않는다면 네비게이션 컨트롤러 스택의 바로 전 뷰 컨트롤러의 제목으로 자동 생성
  - 네비게이션 컨트롤러 스택중에 최상위 컨트롤러의 leftBarButtonItem을 설정하면 Back버튼 대신 이것이 제공
- Middle Item
  - navigationItem.title, navigationItem.prompt로 접근할 수 있음
  - 커스텀 한 Middle Item을 표시하고 싶으면 navigationItem의 titleView 프로퍼티를 설정해주면됨
  - 아무것도 설정되어 있지 않다면 최종적으로 뷰컨트롤러의 title 프로퍼티가 표시
- Right Item
  - 사용자가 설정하지 않으면 노출이 되지 않음
- ToolBar
  - 네비게이션에서 제공하는 하단 부분
  - Navi.isToolBarHidden = false
- delegate
  - 네비게이션 컨트롤러의 라이프 사이클, 회전처리, 커스텀 트랜지션 애니메이션 관련된 메서드를 제공
  - 특정 뷰 컨트롤러에서만 회전에 대한 예외룰을 적용하고 싶을 때 특정 메서드를 구현하여 제어
  - 또는 UINavigationController를 서브 클래싱하여 supportedInterfaceOrientations를 오버라이딩

## 4. UITabBarController

- Parent- Child관계
- 특정 탭을 선택하면 탭에 연결된 뷰 컨트롤러의 뷰가 표시되며 이전 탭의 뷰가 대체
- 모든 뷰 컨트롤러가 한번에 로드가 되는 것이 아니라 필요할 때 로드
- 탭 바 컨트롤러 안에 네비게이션 컨트롤러 중첩 사용
- 탭 6개 이상 추가하면 4개만 표시되고 나머지는 More로 표시