#### Error Handling

- 어떤 동작이 nil이 return 할 때, 값의 부재는 표현할 수 있지만 그 동작이 실패했다는 것을 표현하기는 부족

- Error handling을 지원함.

- error는 Error protocol을 conform하는 value

- enum은 error을 표현하기 좋음

- ```swift
  enum VendingMachineError: Error{
    case invalidSelection
    case insufficientFunds(coinNeeded: Int)
    case outOfStock
  }
  ```

- error를 발생시킬 때는 throw 구문을 사용

  - ```swift
    throw VendingMachineError,insufficientFunds(coinsNeeded: 5)
    ```

- error을 throw하는 function을 호출 할 때는 앞에 try 구문을 사용

  - ```swift
    try canThrowErrors()
    ```

- Do-catch 문을 이용한 error처리

  - ```swift
    do{
      try expression
    } catch pattern 1{
      statements
    }
    ```

- error를 optional value로 처리

  - error가 발생하면 결과를 nil로 처리하고 싶을 때 try?를 사용하면 return value가 optional이 되면서 error를 optional로 처리할 수 있음