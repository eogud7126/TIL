## 프로퍼티 get, set, didSet, willSet

- Get, set 이 한 세트/ didSet, willSet이 한 세트
- 하나의 프로퍼티에 get, set, didSet, willSet 모두 함께 사용하지 못합니다.



#### get, set

- Gettter, setter와 비슷하지만, 해당 프로퍼티에 직접 붙이는 방식이라는것이 조금 특이하다.

- 기본 문법

  - ```swift
    var myProperty: Int{
      get {
        return myProperty
      }
      set(newVal){
        myProperty = newVal
      }
    }
    print(myProperty)
    myProperty = 123
    //이 방식으로 작성하면 Xcode에서 경고를 줍니다.
    //backing storage가 필요
    
    var _myProperty: Int
    var myProperty: Int{
      get{
        return _myProperty
      }
      set(newVal){
        _myProperty = newVal
      }
    }
    ```

- 용도

  - 1. 프로퍼티에 값이 할당 될 때 적절한 값인지 검증하기 위해
    2. 다른 프로퍼티값에 의존적인 프로퍼티를 관리할 때
    3. 프로퍼티를 private하게 사용하기 위해





#### didSet, willSet

- 프로퍼티 옵저버로 didSet, willSet을 제공. 

- 프로퍼티의 값이 변경되기 직전, 직후를 감지. 따라서 원하는 작업을 수행할 수 있다.

- 기본 문법

  - ```swift
    var myProperty: Int = 10{
      didSet(oldVal){
        //myProperty의 값이 변경된 직후에 호출, oldVal은 변경 전 myProperty의 값
        scoreLabel.text = "Score: \(score)"
        //score값이 바뀔때마다 View의 값을 갱신하는 작업을 따로 해주지 않아도 된다.
      }
      willSet(newVal){
        //myProperty의 값이 변경되기 직전에 호출, newVal은 변경 될 새로운 값
      }
    }
    ```

  - 