## Protocol

- 어떤 작업/기능을 위한 method, property의 interface를 정의하는데 사용
- class, struct, enum에 adopt되며 그 때 해당 type은 protocol에서 요구하는 내용을 실제로 구현
- type이 protocol의 요구사항을 구현하면 그 type은 해당 protocol을 conform한다고 함

#### protocol requirement

- Getter, setter 모두를 가지거나 getter만 가질 수 있음.

- ```swift
  protocol SomeProtocol{
    var mustBeSettable: Int{ get set }
    var doesNotNeedToBeSettable: Int{ get }
  }
  ```

#### protocol type

- protocol은 모든 type과 동일한 동작을 할 수 있음

  - Function/method/initializer의 파라미터나 리턴타입으로 사용가능
  - Constant/variable로 사용가능
  - array/dictionary 같은 container의 item으로 사용가능

  ```swift
  protocol Animal{
      var legs: Int{ get }
  }
  struct Person: Animal{
      var legs: Int{
          return 2
      }
  }
  struct Dog: Animal{
      var legs: Int{
          return 4
      }
  }
  let animals: [Animal] = [Person(), Dog()]
  
  for animal in animals{
      print(animal.legs)
  }
  ```



#### Inheritance

- protocol은 기존에 존재하는 다른 protocol을 상속 받아 확장할수 있음

  - ```swift
    protocol InheritingProtocol: SomeProtocol{
      //protocol definition
    }
    ```

- class와는 달리 multiple inheritance가 가능함

  - ```swift
    protocol PrettyTextRepresentable: TextRepresentable{
      var prettyTextualDescription: String{ get }
    }
    
    protocol Named{
      var name: String  { get }
    }
    protocol Age{
      var age: Int{ get }
    }
    
    struct Person: Named,Aged{
      var name: String
      var age: Int
    }
    ```

- 두개 이상의 protocol을 만족하는 value를 파라미터로 사용해야하는 경우

  - ```swift
    func wishHappyBirthday(to celebrator: Named & Aged){
      print("Happy Birthday, \(celebrator.name), you're \(celebrating.age)!")
    }
    
    let birthdayPerson = Person(name: "Dae", age: 21)
    wishHappyBirthday(to: birthdayPerson)
    ```

#### Protocol Extension

- protocol도 extension을 통해 확장이 가능함

- protocol 자체의 기능만 이용해서 기능을 확장

- 기존에 그 protocol을 conform하는 모든 type이 별도의 추가 작업 없이 확장된 기능을 가지게 됨

- ```swift
  extension RandomNumberGenerator{
    func randomBool() -> Bool{
      return random() > 0.5
    }
  }
  ```