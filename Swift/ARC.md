## ARC

- 참조 타입은 하나의 인스턴스가 참조를 통해 여러 곳에서 접근하기 때문에 언제 메모리에서 해제되는지가 중요.

- ARC는 자동으로 메모리를 관리해주는 방식. 더이상 필요하지 않은 클래스의 인스턴스를 메모리에서 해제하는 방식으로 동작

- GC와 차이점

  - | 메모리 관리기법  | ARC                                                          | GC                                                           |
    | :--------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | 참조 카운팅 시점 | 컴파일 시                                                    | 프로그램 동작 중                                             |
    | 장점             | - 컴파일 당시 이미 인스턴스의 해제 시점이 정해져 있어서 인스턴스가 언제 메모리에서 해제될지 예측할 수 있다.<br />- 컴파일 당시 이미 인스턴스의 해제시점이 정해져 있어서 메모리 관리를 위한 시스템 자원을 추가할 필요가 없다 | - 상호 참조 상황등의 복잡한 상황에서도 인스턴스를 해제할 수 있는 가능성이 더 높다.<br />- 특별히 규칙에 신경 쓸 필요가 없다 |
    | 단점             | - ARC의 작동 규칙을 모르고 사용하면 인스턴스가 메모리에서 영원히 해제되지 않을 가능성이 있다. | - 프로그램 동작 외에 메모리 감시를 위한 추가 자원이 필요하므로 한정적인 자원 환경에서는 성능 저하 발생<br />- 명확한 규칙이 없기 때문에 언제 인스턴스가 메모리에서 해제될 지 모른다. |



### 강한 참조

- 인스턴스가 계속해서 메모리에 남아있어야 하는 명분을 만들어 주는 것.
- 인스턴스를 다른 인스턴스의 프로퍼티나 변수 등에 할당 할 때 참조 횟수가 1 증가. Nil 할당시 1 감소.
- 별도로 명시하지 않으면 강한 참조를 한다.

```swift
class Person{
    let name: String
    init(name: String){
        self.name = name
        print("\(name) is being initialized")
    }
    deinit{
        print("\(name) is being deinitialized")
    }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "Dae Hyung")
//Dae Hyung is being initialized"

reference2 = reference1 //인스턴스 참조횟수 2
reference3 = reference1 //인스턴스 참조횟수 3

reference1 = nil    //인스턴스 참조횟수 2
reference1 = nil    //인스턴스 참조횟수 1
reference1 = nil    //인스턴스 참조횟수 0

```



- 강한참조 순환 문제

  - 인스턴스끼리 서로가 서로를 강한 참조할 때

  - 수동 해결법

    - ```swift
      var dae: Person? = Person(name: "daehyung")//Person 인스턴스의 참조횟수 1
      var room: Room? = Room(number: "505") //Room인스턴스의 참조 횟수: 1
      
      room?.host = dae    //Person 인스턴스 참조회수 2
      dae?.room = room    //Room 인스턴스 참조회수 2
      
      dae?.room = nil //Room 인스턴스 참조회수 1
      dae = nil //Person 인스턴스 참조회수 1
      
      room?.host = nil //Person인스턴스 참조회수 0
      room = nil //Room인스턴스 참조회수 0
      
      ```

    - 이런식으로 수동으로 해제할 수 있지만 실수로 인해 코드의 누락이 있을 수도 있다.

  - 약한 참조

    - 참조하는 인스턴스의 참조 횟수를 증가시키지 않는다. Weak 키워드를 통해 프로퍼티나 변수는 자신이 참조하는 인스턴스를 약한 참조한다.

    - ```swift 
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
      ```

  - 무소유 참조

    - 약한 참조와 같이 인스턴스 참조 횟수를 증가시키지 않는다. 

    - 약한 참조와는 다르게 자신이 참조하는 인스턴스가 메모리에서 해제되더라도 스스로 nil을 할당해주지 않는다. 따라서 미소유 참조를 하는 변수나 프로퍼티는 옵셔널이나 변수가 아니어도 된다.

    - 미소유 참조를 하면서 메모리에서 해제된 인스턴스에 접근하려한다면 런타임 오류가 발생한다.

    - 따라서, 미소유 참조는 참조하는 동안 해당 인스턴스가 메모리에서 해제되지 않으리라는 확신이 있을 때만 사용한다.

    - ```swift
      class Person{
          let name: String
          // 카드를 소지할 수도 , 소지 하지 않을 수도 있기 때문에 옵셔널로 정의
          // 또, 카드를 한 번 가진 후 잃어버리면 안되기 때문에 강한 참조를 해야한다.
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
          unowned let owner: Person //카드는 소유자가 분명해야한다.
          init(number: UInt, owner: Person){
              self.number = number
              self.owner = owner
          }
          
          deinit {
              print("\(number) is being deinitialized")
          }
      }
      
      //person 인스턴스 참조횟수 1
      var daehyung: Person? = Person(name: "daehyung")
      
      if let person = daehyung{
          //CreditCard 인스턴스 참조횟수 1
          person.card = CreditCard(number: 1004, owner: person)
          //person 인스턴스 참조횟수 1
      
      }
      daehyung = nil //Person 인스턴스 참조 횟수 0
      //CreditCard 인스턴스 참조횟수 0.
      //daehyung is being deinitialized
      //1004 is being deinitialized
      
      ```

  - 미소유참조와 암시적 추출 옵셔널 프로퍼티

    - ```swift
      class Company{
          let name: String
          
          var ceo: CEO!
          init(name: String, ceoName: String){
              self.name = name
              self.ceo = CEO(name: ceoName, company: self)
          }
          
          func introduce(){
              print("\(name)의 CEO는 \(ceo.name)입니다.")
          }
      }
      
      class CEO{
          let name: String
          //미소유 참조 상수 프로퍼티(미소유 참조)
          unowned let company: Company
          init(name: String, company: Company){
              self.name = name
              self.company = company
          }
          
          func introduce(){
              print("\(name)는 \(company.name)의 CEO입니다.")
          }
          
      }
      
      
      let company: Company = Company(name: "무한상사", ceoName: "김태호")
      company.introduce()
      company.ceo.introduce()
      //무한상사의 CEO는 김태호입니다.
      //김태호는 무한상사의 CEO입니다.
      
      
      ```

    - Company 타입의 인스턴스를 초기화할 때, CEO 타입의 인스턴스를 생성하는 과정에서 자기 자신을 참조하도록 보내줘야하기 떄문에 암시적 추출 옵셔널을 사용해야한다.

    - 즉, 암시적 추출 옵셔널이 아닌 일반 프로퍼티를 사용했다면 자신의 초기화가 끝난 후에만 CEO(name: codeName, company: self)와 같은 코드를 호출할 수 있다는 뜻.

  - 클로저의 강한참조 순환

    - Self.someProperty처럼  인스턴스의 프로퍼티에 접근할 때나 클로저 내부에서 self.someMethod()처럼 인스턴스의 메서드를 호출할 때 값 획득이 발생할 수 있는데, 두 경우 모두 클로저가 self를 획득하므로 강한참조 순환이 발생한다.

    - 그 이유는 클로저가 클래스와 같은 참조 타입이기 때문.

    - 이러한 문제는 클로저의 캡쳐리스트를 통해 해결가능하다.

    - ```swift
      class Person{
          let name: String
          let hobby: String?
          
          lazy var introduce: () -> String = {
          var introduction: String = "My name is \(self.name)"
          guard let hobby = self.hobby else{
              return introduction
              }
          
          introduction += " "
          introduction += "My hobby is \(hobby)"
          return introduction
          }
          
          init(name: String, hobby: String? = nil){
              self.name = name
              self.hobby = hobby
          }
          deinit{
              print("\(name) is being deinitialized")
          }
      }
      
      var daehyung: Person? = Person(name: "daehyung", hobby: "soccer")
      print(daehyung?.introduce())
      daehyung = nil
      
      //Optional("My name is daehyung My hobby is soccer")
      //deinit이 호출되지 않음.. -> 메모리 누수
      
      ```

    - 클로저는 자신이 호출되면 언제든지 자신 내부의 참조들을 사용할 수 있도록 참조 횟수를 증가시켜 메모리에서 해제되는 것을 방지하는데, 이 때 자신을 프로퍼티로 갖는 인스턴스의 참조횟수도 증가시킨다.

  - 앞의 문제를 획득목록(Capture list)을 통해 해결한다. 

    - 획득 목록은 클로저 내부에서 참조 타입을 획득하는 규칙을 제시해줄 수 있는 기능.

    - 획득 목록을 사용하면 때에 따라서, 혹은 각 관계에 따라서 참조 방식을 클로저에 제안할 수 있다.

    - 참조 방식과 참조할 대상을 대괄호([])로 둘러싼 목록 형식으로 작성하고, 획득목록 뒤에는 in 키워드를 써준다.

    - 획득목록에 명시한 요소가 참조타입이 아니라면 해당 요소들은 클로저가 생성될 때 초기화된다.

    - ```swift
      var a = 0
      var b = 0
      let closure = { [a] in
                    print(a, b)
                    b = 20
                    }
      a = 10
      b = 10
      closure() // 0 10
      print(b) //20
      ```

    - 변수 a는 클로저의 획득 목록을 통해 클로저가 생성될 때 값 0을 획득했지만, b는 따로 획득하지 않았다.

    - a 변수는 클로저가 생성되는 동시에 획득목록 내에서 다시 a라는 이름의 상수로 초기화 된 것.

    - 그렇기에 외부에서 a의 값을 변경하더라도 클로저의 획득목록을 통한 a와는 별개가 된다.

    - 그러나 b는 내외부 상관 없이 값이 바뀐다.

    - 획득목록에 해당하는 요소가 참조 타입일 경우

      - ```swift
        class SimpleClass{
            var value: Int = 0
        }
        
        var x = SimpleClass()
        var y = SimpleClass()
        
        let closure = { [x] in
            print(x.value, y.value)
        }
        
        x.value = 10
        y.value = 10
        
        closure()
        
        //10 10
        ```

    - 참조 타입은 획득 목록에서 어떤 방식으로 참조할 것인지: 강한획득, 약한 획득, 미소유 획득. 정할 수 있다. 약한 획득을 할 경우 획득하는 상수가 옵셔널 상수로 지정된다.

    - ```swift
      class SimpleClass{
          var value: Int = 0
      }
      
      var x: SimpleClass? = SimpleClass()
      var y = SimpleClass()
      
      let closure = { [weak x, unowned y] in
          print(x?.value, y.value)
      }
      
      
      x = nil
      y.value = 10
      
      closure()
      //nil 10
      ```

    - x는 약한참조, y는 미소유참조. x가 약한 참조를 하게 되므로 클로저 내부에서 사용하더라도 클로저는 x가 참조하는 인스턴스의 참조 횟수를 증가시키지 않는다. x가 참조하는 인스턴스가 메모리에서 해제되어 클로저 내부에서도 더 이상 참조가 불가능하다.

    - y는 미소유 참조를 했기 때문에 클로저가 참조횟수를 증가시키지 않지만, 만약 메모리에서 해제된 상태에서 사용하려한다면 앱이 강제 종료 된다.

  - 그래서 강한 참조 순환 문제를 어떻게 해결하나??

    - ```swift
      
      error: Execution was interrupted, reason: signal SIGABRT.
      var daehyung: Person? = Person(name: "DaeHyung", hobby: "soccer")
      print(daehyung?.introduce())
      daehyung = nil
      
      //Optional("My name is DaeHyung My hobby is soccer")
      //DaeHyung is being deinitialized -> 메모리 해제
      ```

    - self를 미소유참조를 한 이유는 해당 인스턴스가 존재하지 않는다면 프로퍼티도 호출할 수 없으므로 self는 미소유 참조를 하더라도 실행 중에 오류를 발생시킬 가능성이 없다고 볼 수 있기 때문

    - 단, self를 미소유참조로 지정했을 때 프로퍼티로 사용하던 클로저를 다른 곳에서 참조하게 된 후 인스턴스가 메모리에서 해제되었을 때 문제가 될 수 있다. 그럴 소지가 있다면 weak를 써야한다.

      - ```swift
        class Person{
            let name: String
            let hobby: String?
            
            lazy var introduce: () -> String = { [unowned self] in
                var introduction: String = "My name is \(self.name)"
                
                guard let hobby = self.hobby else{
                    return introduction
                }
                
                introduction += " "
                introduction += "My hobby is \(hobby)"
                return introduction
            }
            
            init(name: String, hobby: String? = nil){
                self.name = name
                self.hobby = hobby
            }
            
            deinit {
                print("\(name) is being deinitialized")
            }
        }
        
        
        var daehyung: Person? = Person(name: "DaeHyung", hobby: "soccer")
        var someone: Person? = Person(name: "some", hobby: "eating")
        
        //someone의 introduce 프로퍼티에  daehyung의  introduce 프로퍼티 클로저의 참조 할당
        someone?.introduce = daehyung?.introduce ?? { " " }
        
        //아직 daehyung이 참조하는 인스턴스가 해제되지 않았기 때문에
        //클로저 내부에서 self(daehyung 변수가 참조하는 인스턴스) 참조 가능
        print(daehyung?.introduce())
        
        daehyung = nil
        
        print(someone?.introduce())	//	오류 발생
        //Optional("My name is DaeHyung My hobby is soccer")
        //DaeHyung is being deinitialized
        
        ```

        - weak를 쓴 경우

          - ```swift
            
            class Person{
                let name: String
                let hobby: String?
                
                lazy var introduce: () -> String = { [weak self] in
                    
                    guard let self = self else{
                        return "원래의 참조 인스턴스가 없어졌습니다."
                    }
                    var introduction: String = "My name is \(self.name)."
                    
                    guard let hobby = self.hobby else{
                        return introduction
                    }
                    
                    introduction += " "
                    introduction += "My hobby is \(hobby)"
                    
                    return introduction
                }
                
                init(name: String, hobby: String? = nil){
                    self.name = name
                    self.hobby = hobby
                }
                
                deinit{
                    print("\(name) is being deinitialized")
                }
            }
            
            
            
            var daehyung: Person? = Person(name: "DaeHyung", hobby: "soccer")
            var someone: Person? = Person(name: "some", hobby: "eating")
            
            someone?.introduce = daehyung?.introduce ?? {" "}
            
            print(daehyung?.introduce())
            
            daehyung = nil
            print(someone?.introduce())
            
            //Optional("My name is DaeHyung. My hobby is soccer")
            //DaeHyung is being deinitialized
            //Optional("원래의 참조 인스턴스가 없어졌습니다.")
            ```

  

