## Automatic Reference Counting

- Reference Type의 메모리 관리/추적을 Reference Count라는 참조 카운트를 이용해서 함
- Swift에서는 대부분의 경우 ARC가 memory를 알아서 관리해주기 때문에 고민할 필요는 없다.
- 일부 code간의 relationship에 관련된 부분으로 인해 compiler에게 조금의 정보를 더 제공함.

```swift
class Person{
    let name: String
    init(name: String){
        self.name = name
        print("\(self.name) is being initialized")
    }
    deinit{//자동으로 메모리 관리
        print("\(self.name) is being deinitialized")
    }
}


var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "John Appleseed")
reference2 = reference1
reference3 = reference1
//위의 코드가 강한 참조


reference1 = nil
print("ref 1 = nil")
reference2 = nil
print("ref 2 = nil")
reference3 = nil
print("ref 3 = nil")

//출력:
/*
John Appleseed is being initialized
ref 1 = nil
ref 2 = nil
ref 3 = nil
John Appleseed is being deinitialized
*/
```

 