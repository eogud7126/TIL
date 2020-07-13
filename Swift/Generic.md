## Generic

- 제네릭을 이용해 코드를 구현하면 어떤 타입에도 유연하게 대응할 수 있습니다.
- 제네릭을 사용한 타입은 재사용이 쉽고 중복을 줄여줍니다.
- Array, Set, Dictionary 모두 제네릭 타입입니다.

```swift
//제네릭을 사용하지 않았을 때
func swapTwoInts(_ a: inout Int, _ b: inout Int){
    let temporaryA: Int = a
    a = b
    b = temporaryA
}

var numberOne: Int = 5
var numberTwo: Int = 10

swapTwoInts(&numberOne, &numberTwo)
print("\(numberOne), \(numberTwo)") // 10, 5



```

- 위 코드는 Int 자료형만 Swap할 수 있다
  - 그렇다면 Any로 바꾸면 되지 않을까??
    - swap하는데에는 무리는 없지만, Any 타입의 두 변수에 어떤 값이 들어있는지 모르기에 Int면 Int끼리 String이면 String 끼리 교환하고 싶을 때, 그런 제한을 줄 수 없다.
    - 또, 전달인자의 타입은 Any로 전달되어야 하는데, 다른 타입인 String이나 Int타입의 변수를 전달인자로 전달할 수 없다. 그래서 String 타입값을 Any 타입의 변수에 넣어서 함수를 호출해야하는데, 이때 값을 복사하여 할당한다. 즉, 새로운 변수로만 함수를 호출할 수 있게 된다.
- 따라서, 제네릭함수를 쓰게 된다면

```swift
//제네릭을 사용할 때
func swapTwoValues<T>(_ a: inout T, _ b: inout T){
    let temporaryA: T = a
    a = b
    b = temporaryA
}
var stringOne: String = "abc"
var stringTwo: String = "def"

swapTwoValues(&numberOne, &numberTwo)
swapTwoValues(&stringOne, &stringTwo)

numberOne
numberTwo
stringOne
stringTwo

//모든 Type에서 Swap할 수 있다.
```

