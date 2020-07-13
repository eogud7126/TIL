## Extension



- 구조체, 클래스, 열거형, 프로토콜 타입에 새로운 기능을 추가할 수 있습니다.
- 익스텐션은 타입에 새로운 기능을 추가할 수는 있지만, 기존에 존재하는 기능을 재정의할 수는 없습니다.



### 상속과 익스텐션 비교

1. 상속
   - 클래스 타입에서만 가능.
   - 특정 타입을 물려받아 하나의 새로운 타입을 정의하고 추가 기능을 구현하는 수직 확장.
   - 재정의 가능
2. 익스텐션
   - 구조체, 클래스, 프로토콜 등에 적용 가능
   - 기존의 타입에 기능을 추가하는 수평 확장
   - 재정의 불가능



### 문법

```swift
extension 확장할 타입 이름: 프로토콜1, 프로토콜2{
  //타입에 추가될 새로운 기능 구현
}
```



```swift
//연산 프로퍼티
extension Int {
    var isEven: Bool{
        return self % 2 == 0
    }
    var isOdd: Bool{
        return self % 2 == 1
    }
}

print(1.isOdd)

//메서드 추가

extension Int{
    func multiply(by n: Int) -> Int {
        return self * n
    }
    mutating func multiplySelf(by n: Int){
        self = self.multiply(by: n)
    }
    static func isIntTypeInstance(_ instance: Any) -> Bool{
        return instance is Int
    }
}

print(3.multiply(by: 4))

var number: Int = 3
number.multiplySelf(by: 3)

print(number)

//이니셜라이저

extension String{
    init(intTypeNumber: Int){
        self = "\(intTypeNumber)"
    }
    init(doubleTypeNumber: Double){
        self = "\(doubleTypeNumber)"
    }
}
let stringFromInt: String = String(intTypeNumber: 100)
let stringFromDouble: String = String(doubleTypeNumber: 100.0)

class Person{
    var name: String
    init(name: String){
        self.name = name
    }
}

extension Person{
    convenience init(){
        self.init(name: "Unknown")
    }
}

let somOne: Person = Person()
print(somOne.name)

//서브스크립트

extension String{
    subscript(appendedValue: String) -> String{
        return self + appendedValue
    }
    subscript(repeatCount: UInt) -> String{
        var str: String = ""
        
        for _ in 0..<repeatCount{
            str += self
        }
        return str
    }
}

print("abc"["def"])
print("abc"[3])

```

