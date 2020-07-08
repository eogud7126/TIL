## Optional Chaining

- optional을 포함한 상태에서 property 조회, method 호출, subscript를 사용하는 방법
- optional이 값을 가지고 있다면 해당 동작은 성공
- 가지고 있지 않다면 nil을 리턴
- 중간에 하나라도 실패하면(nil이면) 전체 결과는 nil
- 결과는 항상 optional

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms{
  print("John's residence has \(roomCount) rooms")
} else{
  print("unable to retrieve the number of rooms")
}
```



#### Optional Chaining으로 function call 하기

- void를 return 하는 function은 optional chaining을 이용하면 Void?를 return

- function call 성공 유무를 if로 확인 가능.

  - ```swift
    func printNumberOfRooms(){
      print("The number of rooms is \(numberOfRooms)")
    }
    if john.residence?.printNumberOfRooms() != nil{
      print("It was possible to print the number of room")
    }
    else{
      print("It was not possible to print the number of rooms")
    }
    ```