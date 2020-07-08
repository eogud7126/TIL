## lazy Variables

- 스위프트에서 메모리는 굉장히 예민한 주제이다.
- 메모리와 관련된 문법

***A lazy stored property is a property whose initial value is not calculated until the first time it is used\***

- lazy 변수는 처음 사용되기 전까지 연산이 되지 않는다.
- 즉, 처음 사용되기 전까지는 로드되지 않다가 사용하려고 클릭하면 서버로부터 로드된다.
- 고려사항

1. lazy는 반드시 var 와 함께 사용해야한다.
   - 값이 존재하지 않고 이후에 값이 생기는 것이기 때문에 let으로 선언될 수 없다.
2. struct, class
   - 기본적으로 lazy는 struct와 class에서만 사용할 수 있다.
3. Lazy vs Computed Property
   - Computed Property에는 lazy 키워드를 사용할 수 없다.
   - lazy는 처음 사용될 때 메모리에 값을 올리고 그 이후부터는 계속 메모리에 올라온 값을 사용한다.
   - 따라서 사용할 때마다 연산하여 사용하는 Computed Property에는 사용하지 않는다.
4. Lazy and closure
   - lazy에 어떤 특별한 연산을 통해 값을 넣어주기 위해서는 코드 실행 블록인 closure를 사용합니다.
   - class나 struct의 다른 프로퍼티의 값을 lazy 변수에서 사용하기 위해서는 closure내에서 self를 통해 접근이 가능합니다.
   - 기본적으로 일반 변수들은 클래스가 생성된 이후에 접근이 가능하기 때문에 클래스 내의 다른 영역(메소드,일반 프로퍼티) 에서는 self를 통해 접근할 수 없지만 lazy키워드가 붙으면 생성 후 나중에 접근할것이라는 의미이기 때문에 closure내에서 self로 접근이 가능합니다.

```swift
class Person{
    var name: String
    lazy var greeting = {
        "Hello my name is \(self.name)"
    }
    /*
     lazy var greeting: String = {
        return "Hello my name is \(self.name)"
     }()
     
     
     */
    init(name: String){
        self.name = name
    }
    deinit{
        print("bye by")
    }
}

var me: Person?
me = Person(name: "Dae")
print(me?.greeting())
me?.name = "Hyung"
print(me?.greeting())

me = nil

/*
출력:
Optional("Hello my name is Dae")
Optional("Hello my name is Hyung")
*/
```



- 만일 변수가 lazy var greeting: String이 아닌 lazy var greeting()->String으로 클로저 실행의 결과를 담는 것이 아닌 클로저 자체를 담고 있는 변수라면 반드시 [weak self] 를 통해 메모리 누수를 방지해야합니다.

```swift
class Person{
    var name: String
//    lazy var greeting = {
//        "Hello my name is \(self.name)"
//    }

//     lazy var greeting: String = {
//        return "Hello my name is \(self.name)"
//     }()

    lazy var greeting: ()-> String = { [weak self] in
        return "Hello my name is \(((self?.name))!)"
    }
    init(name: String){
        self.name = name
    }
    deinit{
        print("bye by")
    }
}


var me = Person(name: "Dae")
print(me.greeting())
me.name = "Hyung"
print(me.greeting())


```

