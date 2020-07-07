## escaping closure



- 탈출하는 클로져: 무엇으로부터??

- ```swift
  //non escaping
  func performNonEscaping(closure: () -> ()){
      print("start")
      closure()
      print("End")
  }
  
  performNonEscaping {
      print("closure")
  }
  //출력값:
  start
  closure
  End
  
  
  
  //escaping closure
  
  func performEscaping(closure: @escaping () -> ()){
      print("start")
      
      DispatchQueue.main.asyncAfter(deadline: .now() + 3){
          closure()
      }
      
      print("end")
  }
  
  performEscaping {
      print("closure")
  }
  //출력값:
  start
  end
  (3초뒤) closure
  ```



#### escaping closure을 왜 쓰는걸까?

- 파라미터 생명주기와 관련: 함수가 시작되면 파라미터가 생성되었다가 종료되면 사라진다.
- Non escaping을 쓰면 함수가 종료되면 클로져도 종료된다.
- escaping을 쓰면 클로져의 실행이 완료될 때까지 클로져를 제거하지 않는다.(3초 뒤 실행후 자동으로 제거)
- 메모리 에러 없이 정상적으로 실행된다.