## 로그인 구현

- 로그인 버튼 둥글게 만드는법
  - LoginViewController를 커스텀 클래스로 지정한다

- 로그인 버튼 눌렀을 때 TabBar로 이동하는 방법

  - IBAction으로 loginViewController에 연결해주고

    - ```swift
      UIStoryboard.init(name: "Main", bundle: nil)
      ```

  - TabBarController의 ID를 설정한다.

  - 완성된 코드

    - ```swift
          @IBAction func login(_ sender: Any) {
              let storyboard = UIStoryboard.init(name: "Main", bundle: nil)
              let viewController = storyboard.instantiateViewController(identifier: "TabBarController")
              
              present(viewController, animated: true,completion: nil)//modale로 띄워줌
          }
      ```

## ViewController override

- viewDidLoad: 뷰 컨트롤러가 로드될때

- viewWillAppear: 화면이 나타날 때
- viewDidAppear: 화면이 다 나타나고 난 뒤
- viewWillDisappear: 사라지기 직전
- viewDidDisappear: 사라지고 나서