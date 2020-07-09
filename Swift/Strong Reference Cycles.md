## Strong Reference Cycles



```swift
class Person {
    let name: String
    init(name: String){
        self.name = name
    }
    
    var room: Room?
    deinit{
        print("\(name) is being deinitialized")
    }
}

class Room{
    let number: String
    init(number: String){
        self.number = number
    }
    var host: Person?
    
    deinit{
        print("Room \(number) is being deinitialized")
    }
}


var daehyung: Person? = Person(name: "DaeHyung")
var room: Room? = Room(number: "505")

room?.host = daehyung
daehyung?.room = room

daehyung = nil
room = nil

//deinit이 안됨.
```

- 서로를 참조하고 있다 -> 강한순환 참조

- 이런 것을 처리하기 위해 weak이라는 modifier 사용

- 통상 상위에서 하위 개념으로는 strong, 반대로는 weak로 강조

- delegate나 data source 같은 것들은 반드시 weak으로 선언

- Weak 참조는 강한참조와 달리 자신이 참조하는 인스턴스의 참조횟수를 증가시키지 않는다.

  - ```swift
    class Person {
        let name: String
        init(name: String){
            self.name = name
        }
        
        var room: Room?
        deinit{
            print("\(name) is being deinitialized")
        }
    }
    
    class Room{
        let number: String
        init(number: String){
            self.number = number
        }
        weak var host: Person?
        
        deinit{
            print("Room \(number) is being deinitialized")
        }
    }
    
    
    var daehyung: Person? = Person(name: "DaeHyung")
    var room: Room? = Room(number: "505")
    
    room?.host = daehyung
    daehyung?.room = room
    
    daehyung = nil
    room = nil
    //출력
    DaeHyung is being deinitialized
    Room 505 is being deinitialized
    
    ```



### 미소유 참조(unowned Reference)

- 미소유 참조는 약한 참조와 다르게 자신이 참조하는 인스턴스가 항상 메모리에 존재하는 것이라는 전체를 기반으로 동작

- 즉, 자신이 참조하는 인스턴스가 메모리에서 해제되더라도 스스로 nil을 할당해주지 않아도 된다.

- 따라서, 미소유참조를 하는 변수나 프로퍼티는 옵셔널이나 변수가 아니어도 된다.

- 미소유 참조를 하면서 메모리에서 해제된 인스턴스에 접근하려 한다면 잘못된 메모리 접근으로 런타임 오류가 발생해 프로세스가 강제로 종료된다.

- 따라서 미소유 참조는 참조하는 동안 해당 인스턴스가 메모리에서 해제되지 않으리라는 확신이 있을 때만 사용해야된다.

- ```swift
  class Person{
      let name: String
      
      //신용카드를 소유할 수도 안할수도 있다
      var card: CreditCard?
      
      init(name: String){
          self.name = name
      }
      deinit{
          print("\(name) is being deinitialized")
      }
  }
  
  class CreditCard{
      let number: UInt
      //카드의 소유자는 반드시 있어야한다.
      unowned let owner: Person
      init(number: UInt, owner: Person){
          self.number = number
          self.owner = owner
      }
      deinit{
          print("Card #\(number) is being deinitialized")
      }
  }
  
  var daehyung: Person? = Person(name: "DaeHyung")
  
  if let person: Person = daehyung{
      //creditcard 참조횟수 1
      person.card = CreditCard(number: 1004, owner: person)
      //person 참조회수 1
  }
  
  daehyung = nil //person 참조회수 0
  //credit 참조회수 0
  
  //결과값:
  DaeHyung is being deinitialized
  Card #1004 is being deinitialized
  
  ```

  

#### 무작정 weak을 써야할까??(안써도 되는것과 꼭 써야되는것)

- self를 참조하는 클로져를 프로퍼티로 가지고 있는경우 weak를 무조건 써야한다.
- 클로저가 참조하는 객체를 서로서로 갖고 있는경우 무조건 캡쳐리스트 해줘야한다.
- 클로저가 어딘가의 property에 저장될 때 캡쳐리스트 사용. 



##### 언제 weak을 쓰고 언제 unowned를 쓰냐??

- Weak은 capture된 레퍼런스가 nil이 될수 있을때
- unowned는 capture된 레퍼런스가 논리적으로 nil이 될일이 없을때.  

