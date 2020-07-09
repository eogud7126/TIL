## Generic

- 특정 type에 종속되지 않는 코드를 작성할 수 있게 해줌
- 유연하고 재사용성이 높음
- 명확한 의도를 표현할 수 있음
- Collection들도 generic을 이용함

```swift
func swapTwoValues<T>(_ a: inout T,_ b: inout T){
  let temporaryA = a
  a = b
  b = temporaryA
}
```



#### Type Parameters

- Type parameter는 <> 사이에 placeholder type을 지정하고 이름을 할당
- placeholder는 실제 존재하는 type이 아니므로 해당 type을 찾지 않음
- Function의 parameter type이나 return type으로 사용 가능하며 function 내부에서 사용됨
- function이 실제로 사용될 때 type interference를 통해 실제 전달되는 value의 type을 찾아 교체됨
- Type parameter는 한 개 이상 지정 가능하며 ","로 구분하여 명시
- 용도에 따라 적합한 이름을 붙여줌.
  - Dictionary에서는 Key, Value라고, Array에서는 Element라고 표기
  - 특별한 의미가 없을 때는 T,U,V 같은 문자를 이용

#### Associated Type

- protocol을 정의할 때 그 protocol이 사용할 임의의 type을 선언해 둘 수 있음

- associatedtype이라는 keyword를 사용

- ```swift
  protocol Container{
    associatedtype Item	//Item이라는 type이 있다 정도로 정의
    mutating func append(_ item: Item)
    var count: Int{ get }
    subscript(i: Int) -> Item{ get }	//실제로 존재하지 않는 Item을 이용
  }
  ```

  