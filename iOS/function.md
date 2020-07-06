## function 함수

- 파라미터가 없는 함수

```swift
func sayHelloWorld()-> String{
  return "hello, world"
}
print(sayHelloWorld())
```



- 파라미터가 여러개인 함수

```swift
func sayHello(personName: String alreadyGreeted: Bool)->String {//문자열 형태로 반환하겠다는 뜻
	if alreadyGreeted{
    return sayHelloAgain(personName)
  } else {
    return sayHello(personName)
  }
 }
print(sayHello("Tim",alreadyGreeted: true))
```

- 리턴값이 여러개인 함수
  - 여러 개의 값은 하나로 합성 되어 있어야한다(tuple)

```swift
func minMax(array: [int])->(min: Int, max: Int){
  var currentMin = array[0]
  var currentMax = array[0]
  for value in array[1..>array.count]{
		if value < currentMin {
      currentMin = value
    }	else if value > currentMax {
      currentMax = value
    }
  }
  return (currentMin,currentMax)
}
```



- Optional Tuple Return Types
  - 만약 함수의 반환값이 tuple이고, 이 값이 nil일 가능성이 있다면 Optional Tuple타입을 리턴 타입으로 정의
    - (Int, Int)? 와 (Int?, Int?)는 다른 의미이다.

```swift
func minMax(array: [Int])->(min: Int, max: Int)?{
	if array.isEmpty{ return nil }
  var currentMin = array[0]
  var currentMax = array[0]
  for value in array[1..<array.count] {
		if value < currentMin{
      currentMin = value
    }	else if value > currentMax {
				currentMax = value
    }
  }
  return (currentMin, currentMax)
}

if let minMaxTuple = minMax([]){
	print(minMaxTuple.min)
}
else {
  print("Empty!")
}
```



- 파라미터 이름

```swift
func someFunction(firstParameterName: Int, secodParameterName: Int){
  //function body
}
someFunction(1, secondParameterName: 2) //첫번째 파라미터 external name이 생략
//두번째부터는 local namedㅡㄹ 가져야한다.




//external parameter name을 정의하는 방법은 local parameter name의 왼쪽에 스페이스바로 구분하여 명시하는 것.
//external parameter name이 따로 정의되어 있는 파라미터는 함수를 호출할 때 꼭 그 이름을 명시..(첫번째여도)

func someFuction(extenalParameterName localParameterName: Int) {
  //externalParameterName이라는 이름으로 argument를 넘김
}

func sayHello(to person: String, and anotherPerson: String)-> String{
  return "Hello \(person) and \(anotherPerson)"
}
print(sayHello(to: "Bill", and: "Ted"))

//이렇게 해주는 이유는 external name은 함수를 명시적이고 말이 되게끔 호출할 수 있게 만들어준다. local name은 함수 내부 구현을 가독성 있고 의도한 바를 명확하게 보일 수 있게함.
```





- Omitting External Parameter Names
  - 함수를 호출할 때 첫 번째 파라미터는 
