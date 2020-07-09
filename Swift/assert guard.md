## assert

- 디버그 모드에서만 동작하고, 배포된 애플리케이션에서는 제외.
- 주로 디버깅 조건 검증

```swift
var intValue: Int = 0
assert(intValue == 0, "intValue != 0")
intValue = 1
assert(intValue == 0)
assert(intValue == 0, "inValue != 0")
```

- assert의 첫번째 파라미터는 검증할 조건이 들어가고, 실패할 경우 동작이 중지됩니다.
- 두번 째 파라미터는 첫 번째 조건에서 검증이 실패했을 경우 보여주는 메시지이고 생략이 가능합니다.

```swift
func checkAge(age: Int?){
  assert(age != nil,"age==nil")
  assert((age! >= 0) && (age! <= 150),"나이를 0~150사이로 입력해주세요")
    print("입력하신 나이는 \(age)입니다.")
}
checkAge(age: 26)
//checkAge(age: -10)
checkAge(age: nil)

```

---------------



## guard 

- guard를 사용해서 잘못된 값이 전달될 경우 특정 실행 구문을 빠르게 종료.
- assert와는 다르게 배포 후에도 동작
- guard에는 else 블록이 있는데 else 블록 내부에는 특정 코드를 종료하는 break, return 문이 무조건 존재해야합니다.
- 타입 캐스팅 또는 옵셔널과 자주 사용됩니다.

```swift
func example(age: Int?){
    guard let inputAge = age, inputAge<150, inputAge>=0
        else{
        print("나이를 0~150 사이로 입력해주세요")
        return
    }
    print("입력한 나이는 \(inputAge) 입니다.")
}

//while문에서 guard
var countInt: Int = 0
while true {
    guard countInt < 5 else {
        break
    }
    print(countInt)
    countInt += 1
}

//Dictionary에서의 guard
func useBothDictionary(nameNAge: [String: Any]){
    guard let name = nameNAge["name"] as? String else{
        return
    }
    guard let age = nameNAge["age"] as? Int else{
        return
    }
    print("이름:\(name), 나이:\(age)")
}
useBothDictionary(nameNAge: ["name": "al", "age": "26"])
//guard 검증 실패로 건너뜀
useBothDictionary(nameNAge: ["name": "tong"])
//guard 검증 실패로 건너뜀
useBothDictionary(nameNAge: ["name":"mon", "age": 26])

```



