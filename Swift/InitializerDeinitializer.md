## Initialization

- class, struct, enum 을 사용하기 위해 준비하는 과정

- Stored property에 대한 초기화 및 instance를 사용하기 위한 다른 초기화

- initialzer라는 메소드를 이용함

- class의 경우 initializer와 쌍으로 deinitializer를 가질 수 있음

- 기본적인 형태 

  - ```swift
    struct Fahrenheit {
        var temp: Double
        init(){
            temp = 32.0
        }
    }
    
    struct Fahrenheit2{
        var temp = 32.0
    }
    
    var f = Fahrenheit()
    print("The default temperature is \(f.temp)")
    
    ```

- Initialization Parameter 이용

  - ```swift
    struct Celsius{
        var tempC: Double
        
        init(fromFahrenheit fahrenheit: Double){
            tempC = (fahrenheit - 32.0) / 1.8
        }
        init(fromKelvin kelvin: Double){
            tempC = kelvin - 273.15
        }
    }
    // initializer는 별도의 name을 가지지 않으므로 parameter에 따라 어떤 initializer가 호출될지 결정됨.
    struct Color{
        let red, green, blue: Double
        init(red: Double, green: Double, blue: Double){
            self.red = red
            self.green = green
            self.blue = blue
        }
        init(white: Double){
            red = white
            green = white
            blue = white
        }
    }
    
    ```

- Optional Property

  - Optional로 선언되어 있는 property의 경우 initialization시 반드시 초기화 될 필요 없음
    - 초기화하지 않으면 nil로 초기화 됨.

- Memberwise initializer

  - struct의 경우 custom initializer를 만들지 않으면 자동으로 member wise initializer가 만들어짐

  - ```swift
    struct Size{
      var width = 0.0, height = 0.0
    }
    //다음과 같이 정의하지 않은 initializer 를 사용가능
    let twoByTwo = Size(width: 2.0, height: 2.0)
    ```

- Initializer Delegation

  - Initializer는 초기화 과정의 일부를 다른 initializer에게 위임 가능

  - 여러개의 initializer간에 코드가 중복되는 것을 방지 할 수 있음

  - Value type의 경우 상속이 없기 때문에 비교적 단순한 형태의 initializer delegation이 존재

  - ```swift
    //하나의 Initializer가 다른 type 안에 있는 다른 initializer를 이용해서 중복 코드를 제거
    
    struct Rect{
    	var origin = Point()
      var size = Size()
      init(){}
      init(origin: Point, size: Size){
        self.origin = origin
        self.size = size
      }
      init(center: Point, size: Size){
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
      }
    }
    ```

  - Class의 경우 복잡한 initializer delegation이 존재할 수 있음

- Designated Initializer

  - Primary initializer로 모든 property를 초기화 시키는 역할을 함

  - super의 initializer를 사용

  - 모든 class는 하나 이상의 deisignated initializer를 가짐

  - ```swift
    class Person{
      var name: String
      var age: Int
      var gender: String
      init(name: String, age: Int, gender: String){
        self.name = name
        self.age = age
        self.gender = gender
      }
    }
    ```

- Convenience init

  - 보조 이니셜라이져

  - Desinated init이 먼저 선언되어야 사용할 수 있음.

  - 같은 클래스에서 다른 이니셜라이저를 호출해야한다.

  - ```swift
    class Person{
      var name: String
      var age: Int
      var gender: String
      init(name: String, age: Int, gender: String) {
        self.name = name
        self.age = age
        self.gender = gender
      }
      convenience init(age: Int, gender: String){
        self.init(name: "DaeHyung", age: age, gender: gender)
      }
    }
    ```

- Automatic Initializer Inheritance

  - default로 initializer가 상속되지 않지만 특별한 조건이 되면 자동으로 상속됨
  - 만일 상속된 class의 모든 새로 정의된 property에 default value가 있다면 다음과 같은 rule을 따름
    - 1. 만일 subclass가 designated initializer를 가지지 않으면 모든 super class의 designated initializer가 상속됨
      2. 만일 subclass가 superclass의 모든 designated initializer를 구현하면 자동으로 superclass의 모든 convenience initailizer가 상속됨.

- Failable initializer

  - 잘못된 파라미터나 외부 resource가 존재하지 않는 경우

  - Init?을 사용해서 만듦

  - Initialization 실패할 경우 nil을 리턴

  - ```swift
    struct Animal{
      let species: String
      init?(species: String){
        if species.isEmpty{ return nil } //optional animal type으로 반환
        self.species = species
      }
    }
    ```

  - Super class의 failable initializer를 오버라이딩하여 non-bailable initializer로 만들 수 있음

- Required Initializer

  - 모든 subclass들이 구현해야하는 initializer

  - required keyword 사용

  - ```swift
    required init(){
      //init
    }
    ```

- Closure나 function을 이용한 default property value setting

  - ```swift
    struct ChessBoard{
      let boardColors: [Bool] = {
        var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
          for j in 1...8 {
            //temporaryBoard.append(isBlack)
            temporaryBoard[i] = isBlack
            isBlack = !isBlack
          }
            isBlack = !isBlack
        }
    return temporaryBoard
      }()
        func squareisBlackAt(row: Int, column: Int) -> Bool{
            return boardColors[(row * 8) + column]
        }
    }
    
    ```
    
    

