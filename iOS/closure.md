## Closure와 Fucntion

- Global function은 이름이 있고 value를 capture하지 않는 closure
- Nested function은 이름이 있고 그것을 포함하는 function의 value를 capture하는 closure
- Closure는 이름이 없고 그것을 둘러 싼 context의 capture



### Capture란?

- closure(또는 nested function) 코드 밖에서 사용되던 variable이나 constant의 값이 closure 안에서 사용될 수 있는것
- 외부의 값이 더 이상 존재하지 않아도 사용 가능함

```swift
func makeIncrementer(forIncrement amount: Int)->()->Int {
  var runningTotal: Int = 0
  
  func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}
let incrementer: () -> Int = makeIncrementer(forIncrement: 10)
print("result = \(incrementer())")
print("result = \(incrementer())")
```



#### 함수 vs 클로저

- Function
  - 이름이 있다.
  - Func 키워드가 존재한다
  - in 키워드가 존재하지않는다.
- Closure
  - 이름이 없다.
  - Func 키워드가 존재하지 않는다.
  - in 키워드가 존재한다.

```swift
func giveFunc(){...} //function
var giveNoFunc = {()-> in ...}
//call

giveFunc()
giveNoFunc()
```



##### Function To Closure

```swift
func sayHello(name: String)-> String {
  return "Hello \(name)"
}
```



1. 중괄호를 제거

   - ```swift
     func sayHello(name: String) -> String
     	return "Hello \(name)"
     ```

2. in 키워드를 함수의 본체와 반환타입 사이에 추가

   - ```swift
     func sayHello(name: String) in
     	return "Hello \(name)"
     ```

3. Func 키워드와 함수의 이름을 제거

   - ```swift
     (name: String) -> String in
     	return "Hello \(name)"
     ```

4. 전체를 중괄호로 감싼다.

   - ```swift
     {(name: String)-> String in
     	return "Hello \(name)"
     }
     ```

5. 이 클로저를 변수에 할당 후 사용

   - ```swift
     var sayHello = {(name: String) -> String in
                    return "Hello \(name)"
                    }
     sayHello("DaeHyung")
     ```



#### 언제, 왜 사용하는지?

- completion blocks, callbacks 고차함수 등
- 가독성이 좋다는 장점.-> 여러 종류의 축약형

축약형

```swift
0. 기본 문법
var sayHello = {(name: String)-> String in 
               return "Hello \(name)"
               }


1. return 값으로 반환되는 타입을 추측가능
var sayHello = {(name: String) in
               return "Hello \(name)"
}
//클로저에서는 return 키워드 또한 생략할 수 있다. 마지막에 있는 값을 반환 값으로 인식한다.

var sayHello = {(name: String) in
               "Hello \(name)"}

2. 변수의 타입을 명시해주었다면 매개변수의 타입도 생략 가능

var sayHello: (String)-> String = {"Hello \($0)"}
// $ 기호를 통해 매개변수를 0부터 순서대로 접근할 수 있다.
```



#### 전달 인자로서의 클로저

클로저는 주로 함수의 전달인자로 많이 사용됩니다. 함수 내부에서 원하는 코드블록을 실행할 수 있다.



```swift
let add: (Int, Int) -> Int = {$0 + $1}
let sub: (Int, Int) -> Int = {$0 - $1}
let mul: (Int, Int) -> Int = {$0 * $1}
let div: (Int, Int) -> Int = {$0 / $1}

func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int{
    return method(a,b)
}

print(calculate(a: 1, b: 2, method: add))
print(calculate(a: 2, b: 3, method: mul))

```



#### 후행 클로저

- 클로저가 함수의 마지막 전달 인자이거나 전달 인자가 클로저 하나뿐이라면 전달인자로 넘기지 않고 함수를 호출 후 중괄호를 통해 클로저를 구현해줌으로써 인자 전달이 가능합니다.

```swift
result = calculate(a: 3, b: 2) {(left: Int, right: Int) -> Int in
                               return left + right
                               }
//축약

result = calculate(a:3, b:2) {$0 + $1}
```

