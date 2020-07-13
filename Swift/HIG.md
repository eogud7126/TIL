## Human Interface Guidline

- 애플에서 제공해준 UI 권장사항 가이드라인



### Interface Essentials

- Bar- 사용자가 어디에 있는지 말해주는 요소. 정보를 전달하고 action을 초기화하기 위한 버튼
- Views - 사용자들이 볼 주요 컨텐츠들이 표시되는 요소.
- Controls - action을 초기화하고 정보를 전달하는 요소. 버튼 스위치, 텍스트 필드, 진행 바 등
- 위 요소들은 UIKit에 정의되어 있으며, 다른 framework와 유기적으로 작용하여 구현



### App Architecture

- Loading
  - 컨텐츠를 로딩중일 때 화면이 멈춰있으면 사용자에게 혼란을 야기하여 앱을 떠나게 됨.
  - 사용자가 무언가를 하고 있음을 표현하기 위해 Activity Spinner 표시
  - 얼마나 걸릴지 progress를 보여주는게 더 좋음
  - 컨텐츠는 즉시 보여지는 것이 좋다. 사용자에게 바로 컨텐츠를 제공하기 위해 background에서 최대한 미리 로딩을 한다.
- Modality
  - 이전 화면 위에 별도로 임시 컨텐츠를 표시하는 방법
  - 독립된 태스크나 관련된 옵션에 집중하도록 도와준다.
  - 사용자들이 중요한 정보를 받거나 선택하도록 보장한다.
  - 현재의 태스크나 별개의 태스크를 수행하거나 선택하기 위해 사용자의 집중을 유도하는 요소
  - alert는 무언가 잘못됐을 때 알리는 데 사용. 
  - 어디서나 dismiss가 가능하도록 버튼을 구현. 
  - 닫기 전에 데이터의 유실이 될 가능성이 있으면 사용자로부터 확인을 받도록 구현
  - popover위에 카드를 표시하지 않는다. 카드를 표시하기 전에 popover를 닫고 표시
  - modal은 이전과 다른 작업을 수행하기 때문에 modal task의 용도와 정의를 title을 통해 표시
- Navigation
  - Hierarchical navigation
    - 메일, 설정
  - Flat navigation
    - 음악, 앱스토어
  - Content-driven/Experience-driven navigation
    - 게임, books
- Onboarding
  - Launch screen을 제공
  - skip을 통해 빠르게 앱을 진입할 수 있도록해야한다.
- Requesting permission
  - 앱에서 권한이 필요한 이유에 대해서 문구를 제공. 사용자에게 강요하지 않도록 한다. 앱 이름을 포함할 필요는 없다.
  - 앱 동작에 필요한 권한은 시작할 때 요구
- Settings



### User Interaction

- Audio
  - 시스템 볼륨과 별도로 볼륨을 조정 가능
  - MPVolumeView를 통해 시스템 볼륨 화면에 대해 커스텀 가능
  - Interruption이 발생한 뒤에 오디오 재생을 재개
  - 오디오 사용이 완료되면 다른 앱들이 알 수 있게 설정
- Authentication
  - Sign in with Apple을 사용하지 않으면 Password AutoFill을 사용
  - 가능한 Sign-in을 사용하지 않고도 앱을 이용할 수 있도록 구현
  - 가입이 필요한 서비스에서는 그에 따른 혜택을 설명
  - 각 항목에 대해서 적절한 키보드 타입을 선택하여 표시
  - passcode는 iOS잠금해제의 대명사이기에 단어 사용금지
- Data Entry
  - 가능한 시스템으로 정보를 얻어, 사용자의 입력을 최소화
  - 입력하는 즉시 validation 체크 수행
  - 필요할 때만 데이터를 입력하도록 한다.
  - 자주 입력해야되는 것은 default value
- File Handling
  - 파일이 기록되는 중에 얼마든지 취소/삭제가 가능해야한다.
  - 파일을 로컬에만 저장하는 옵션을 제공하지 마라. Cloud base로 이용 가능하게 해서 모든 기기에서 확인 가능토록.
  - Quick look을 통해 파일을 열지 않고도 미리 볼 수 있도록.
- Gestures
  - 시스템에서 제공하는 표준 제스처를 사용
  - OS에서 제공하는 시스템 전반적인 제스처를 방해하지 마라
  - Multi-finger 제스처에 대해서 항상 고려



### Visual Design

- Adaptivity & Layout
  - 모든 Tap이 가능한 Control은 44x44 이상의 크기를 지녀야한다.
  - Preview를 사용해서 다양한 디바이스를 확인
  - Control은 너무 바닥이나 코너에 두지 말아라.
  - Safe-Area 



### Icons & Image

- Resolution
  - Rendered Pixels(pt)
    - 다양한 물리적 기기에 동일한 시각 요소를 제공하기 위해 시스템에서 사용되는 좌표계
  - Physical Pixels(px)
    - 실제 물리적 디스플레이에서 사용되는 좌표계
- App Icon & custom Icon
  - PNG 포맷
- Search Bars
  - Text field보다 Search Bar를 이용해서 검색 구현 권장
  - Search bar에 어떤 내용을 검색할 것인지 hint 추가
  - search bar 하단에 유용한 정보나 기능들을 제공
- Tab Bar
  - App level에서 보여지는 저옵의 분류에 대해 Tab Bar 사용
  - 화면의 요소를 바꾸는 것이 아니라 화면을 전환하는 요소로 사용
  - 화면의 요소를 바꾸기 위해 Tool bar를 사용
  - 많은 Tab을 만들지마라
- Scroll Views
  - Scroll view사용할 때, 적적히 Zoom기능을 지원
  - Paging 기능을 사용하면 PageControl을 사용할 것은 고려
  - Scroll View 안에서 다른 Scroll을 놓지마라
  - 일반적으로 한번에 하나의 Scroll View만 보여줘라
- Collections & Table
  - Visual layout을 구현할 때 Collection View를 사용
  - Textual information은 TableView를 사용
  - 표시하기 복잡해서 refresh가 오래 걸리는 데이터는 cell의 대사용에 의해서 잘못 표시될 수 있다.
  - Cell은 앱의 디자인에 맞추어 customizing해서 사용
- Progress indicators 
  - 작업을 수치로 환산 가능한 경우 progress bar 사용, 불가능하면 Activity indicator 사용
  - 진행중인 작업에 대한 정보를 제공하는 것을 선호
  - Navigation bar나  Toolbar에서 progressbar를 사용할 때는 unfilled portion을 숨겨라
  - 수 초 이상 걸리는 작업에 한해서 보여줘라