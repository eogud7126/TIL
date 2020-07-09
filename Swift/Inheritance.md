## Inheitance

- class는 다른 class의 method, property들을 상속 받을 수 있음.
  - class만
- subclass는  superclass의 method, property, subscript를 overriding 할 수 있음.
- subclass는 superclass의 property를 observing 할 수 있음

-----------



## overriding method

- 기존의 method에 기능을 추가하거나 변경할 수 있음

```swift
class Train: Vehicle{
  override func makeNoise(){
    print("Choo Choo")
  }
}
```



### Superclass의 method, property, subscript 사용하기

- Overriding 했을 때, superclass의 이미 존재하는 구현을 사용해야할 때가 있음.
  - method의 경우: super.method()
  - property의 경우: super.property
  - subscript의 경우: super[index]

