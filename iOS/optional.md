- ~~~swift
  ## Optional

  - 값이 존재하지 않을 수 있을 때 사용.
  - ?로 표현 Int?
  - 값이 없는 경우 optional 변수에 nil을 넣음
  
        var responseCode: Int? = nil // 값을 선언하지 않으면 default 값이 nil
        var message: String? //nil
  
  - if문을 이용하여 optional 변수의 값의 존재 유무를 확인 가능
  - 값의 존재가 확실한 optional 변수에 대해서 변수 뒤에 "!"를 붙엿 강제로 값을 사용하게 할 수 있음.
  
          if convertedNumber != nil {
            print("convertedNumber has an integer value of \(convertedNumber)")
          }
          
   > 현업에서는 사용하지 않음 런타임 에러 가능성
   
   이를 해결하기 위해 Optional Binding 을 사용
   - Optional 변수에 값이 있는지르 확인해서 있는 경우 상수에 넣어서 사용하는 것은 안전학 코딩할 수 있는 중요한 방법이며 이를 위한 문법을 제공
   
          var possibleNumber: String = "..."
          if let actualNumber = Int(possibleNumber){  //새로운 변수에 값으 넣었을 때 값이 있으면
            print("The string \"\(possibleNumber)\ has an intenger value of \(actualNumber)"
            }
            else{
            print("The string \"\(possibleNumber)\" could not be converted to an integer")
            }
            //Prints "The string "123" has an integer value of 123'
            
            //다음과 같이 확장할 수 있다.
            
            if let firstNumber = Int("4), let secondNumber = Int("42"), firstNumber < secondeNumber
            && secondNumber < 100 {
              print("|(firstNumber) < \(secondNumber) < 100")
            }
          //Prints "4<42<100
          
  - optional binding 과 관련된 early exit을 필욜 할 때에는 guard let문을 이용
  
          guard let name = optionalName, name.count > 5 else {
              retrun
          }
          
  - guard-let
    - 조건을 만족시키지 못할경우 해당 code block을 실행하지 않아야할때. 즉, early exit 필요할 때.
  
  - If-let
    - 조건의 만족 유무를 떠나 code block이 계속 진행되어야 할 때
  
  - guard를 무시한채 쓰면 버그 발생 가능성 있음.
  
  
  
  implicitly Unwrapped Optionals
  
  ---------------------------
  
  - Optional이기는 하지만 값이 있다고 여김.
  - Optional이지만 일반 변수처럼 사용.
  
  -----------------
  
  
  
  - Optional은 특정 type의 값을 저장하는 box라고 생각할 수 있다.
  
  - Optional Int는 Int를 담을 수 있는 상자를 뜻함 -> 안이 비어있을 수도, 어떤 값이 있을수도 있음
  
  - Unwrap은 box의 포장을 벗겨서 실제 내용을 가져오는 동작
  
    
  
  ------------------
  
  ### Swift는 어떻게 코딩?
  
  - 매우 강력한 type checkaing 문법을 가진 언어이므로 항상 type에 신경.
  - var의 사용을 최소화, let의 사용을 늘려야함(let이 const이므로
  - 값의 부재가능성이 있는지 없는지 판단해야함.(부재 가능성이 없으면 optional X)
  
  
  
  --------------------
  
  
  
  반복문
  
  
  
  - for - in
  
    > sequence를 iterate
    >
    > Sequence protocol을 만족하는 모든 객체에서 사용
    >
    > Sequence란.. 루프로 된 반복되는 타입?
  
  ​~~~swift
  ```
  let names = ["Anna", "Alex", "Brian", "Jack"]
  for name in names {
    print("Hello, \(name)!")
  }
  let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
  for(animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
  }
  
  for i in 0...102 where(i % 3) == 0{
      print("3의 배수")
  }
  ​~~~
  
  
  
  - switch문
  
  > Patten matching
  >
  > 복잡한 조건에 따른 분기 가능
  >
  > break문을 쓰지 않아도 각 case만 실행
  >
  > 모든 case문 (default 포함)은 반드시 실행문이 한 줄 이상 있어야함.
  >
  > 모든 경우에 대한 case문이 존재하지 않으면 반드시 default 필요함.
  
  ```swift
  let someCharacter: Character = "z"
  switch someCharacter {
    case "a", "e", "i", "o", "u" :
    	print("\(someCharacter) is a vowel")
    case "z" :
    	print("The last letter of the alphabet")
    default:
    	print("\(someCharacter) is not a vowel or a last letter")
  }
  
  //또는 value binding
  //값을 case문 안에서 특별한 변수/상수에 binding해서 사용하는 것이 가능
  
  let anotherPoint = (2,0)
  switch anotherPoint {
    case (let x, 0):
    	print("on the x-axis with an x value of \(x)")
    case (0, let y):
    	print("on the y-axis with an y value of \(y)")
    case let(x, y):
    	print("somewhere else at (\(x),\(y))")
  }
  
  //case문 실행 후 바로 아래 case문까지 실행하는 것이 필요하다면 fallthrough 문 사용
  //아래 case 문은 조건에 맞지 않아도 실행
  ```
  
  
  
  - 조건문에 label 붙이기(Labeled statements)
  
    > continue와 break는 labeled statements의 경우 한번에 여러 statements를 벗어날 수 있게 해줌
  
  - where문에서는 루프문에서 추가적인 조건을 확인 할 수 있다.
  
  
  
  ---------------
  
  ## Nil Coalescing Operator
  
  - 만일 값이 있으면 unwrap하고 없으면 다른 값을 사용
  
    ```swift
    a ?? b
    a != nil ? a! : b
    if let a = a {
      return a
    }
    else {
      return b
    }
    //위의 두 구문과 내용이 동일하다.
    ```
  
    
  
  --------------
  
  ### Collection
  
  > Value type
  >
  > 다른 변수에 대입하면 복사됨
  
  - Array
    - Ordered collection of values
    - 기본 표현
      - let array: Array<SomeType>()
    - 간략한 표현
      - let array: [SomeType]
  
  ```swift
  //Array 초기화
  //초기화 구문을 이용한 기본적인 초기화
  var someInts = [Int]()
  var someInts = Array<Int>()
  
  // 동일한 값으로 채워서 초기화
  var doubles = [Double](repeating: 0.0, count: 1)
  
  // 두 Array를 이용해서 새 Array 초기화
  var doubles1 = [Double](repeating: 0.0, count: 3)
  var doubles2 = [Double](repeating: 1.0, count: 3)
  var doubles3 = doubles1 + doubles2
  
  //Literals를 이용한 초기화
  var shoppingList: [String] = ["Eggs", "Milk"]
  ```
  
  
  
  - Set 
  
    - unordered collection of district values
    - 기본 표현
      - let letters = Set<Character>()
    - 꼭 타입을 명시해줘야함.
    - set을 집합으로 생각하면 됨. 여러가지 연산 제공
  
  - Dictionary
  
    - Unordered collection of key-value associations
  
    - 순서 없이 저장
  
    - key의 타입은 Hashable이어야함.
  
    - 기본 표현
  
      - Dictionary<KeyType, ValueType>
  
    - 간략한 표현
  
      - [KeyType: ValueType]
  
    - 리턴값이 항상 옵셔널이다.
  
      ```swift
      if let airport: String? = airports["ICN"]{
      	//값이 있음
      }
      else {
        //값이 없음
      }
      ```
  
      
  ~~~
