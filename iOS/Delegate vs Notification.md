## Notification과 Delegate 차이

#### 공통점

- View와 ViewController 간, 또는 각각의 것들 사이에서 소통이 필요할 때 사용
- 하나의 객체가 다른 객체와 소통은 하지만, 묶이기는 싫을 때..
  - ViewController는 한 View를 관리하는 독립적인 객체로서 존재해야한다.





#### Delegate

- 보통 protocol을 정의하여 사용한다. 

- ```swift
  protocol SomeDelegate{
    func somFunc(someParam: String)
  }
  
  class SomeView: UIView{
    weak var delegate: SomeDelegate?
    
    func someTapped(str: String){
      delegate?.someFunc(someParam: str)
    }
  }
  
  
  class SomeController: SomeDelegaet {
    var view: SomeView?
    
    init(){
      view = SomeView?
      view = SomeView()
        
      view?.delegate = self
      someFunc(someParam: "Hi")
  	}
      
    func someFunc(someParam: String){
      print(someParam)
    }
  }
  let someController = someController()
  
  ```

- 장점

  - 매우 엄격한 Syntax로 인해 프로토클에 필요한 메소드들이 명확하게 명시됨.
  - 컴파일 시 경고나 에러가 떠서 프로토콜의 구현되지 않은 메소드를 알려줌,
  - 로직의 흐름을 따라가기 쉬움
  - 프로토콜 메소드로 알려주는 것 뿐만 아니라 정보를 받을 수 있음.
  - 커뮤니케이션 과정을 유지하고 모니터링하는 제 3의 객체가 필요 없음.
  - 프로토콜이 컨트롤러의 범위 안에서 정의됨

- 단점

  - 많은 줄의 코드가 필요
  - Delegate 설정에 nil이 들어가지 않게 주의.
  - 많은 객체들에게 이벤트를 알려주는 것이 어렵고, 비효율적



#### Notification

- Notification Center라는 싱글톤 객체를 통해서 이벤트들의 발생 여부를 옵저버를 등록한 객체들에게 Notification을 post하는 방식으로 사용한다. Notification Name이라는 키값을 통해 보내고 받을 수 있다.

- ```swift
  //Noti Sender
  class PostViewController: UIViewController{
    @IBOutlet var sendNotificationButton: UIButton!
    
    @IBAction func sendNotificationTapped(_ sender: UIButton){
      guard let backgroundColor = view.backgroundColor else { return }
      NotificationCenter.default.post(name: Notification.Name("notification"), object: sendNotificationButton, userInfo: ["backgroundColor": backgroundColor])
     
    }
  }
  
  //Noti Recieve
  class ViewController: UIViewController {
    override func viewDidLoad(){
      super.viewDidLoad()
      
      //옵저버 추가
      NotificationCenter.default.addObserver(self, selector: #selector(notificationRecieved(notification: )), name: Notification.Name("notification"), object: nil)
    }
    
    @objc func notificationReceived(notification: Notification) {
      guard let notificationObject = notification.object as? UIButton else{ return }
      print(notificationObject.titleLabel?.text ?? "text is empty")
      
      guard let notificationUserInfo = notification.userInfo as? [String: UIColor], let postViewBackgroundColor = notificationUserInfo["backgroundColor"] else{ return }
      print(postViewBackgroundColor)
    }
  }
  ```

- 장점

  - 많은 줄의 코드가 필요 없이 쉽게 구현 가능
  - 다수의 객체들에게 동시에 이벤트의 발생을 알려줄 수 있음
  - Notification과 관련된 정보를 Any? 타입의 object, [AnyHashable: Any]? 타입의  userInfo로 전달할 수 있음.

- 단점

  - key값으로  Notification의 이름과 userInfo를 서로 맞추기 때문에 컴파일시 구독이 잘 되고 있는지, 올바르게 userInfo의 value를 받아오는지 체크가 불가능함.
  - 추적이 쉽지 않을 수 있음
  - Notification post 이후 정보를 받을 수 없음.



