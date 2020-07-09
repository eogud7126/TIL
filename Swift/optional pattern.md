## optional pattern



```swift
let a: Int? = 0 //단축 문법
let b: Optional<Int> = 0    //원래 문법
//------------------------------
if a == nil {
    
}
//이거 2개 같고
if a == .none{
    
}
//------------------------------
if a == 0 {
    
}
//이거 2개 같다
if a == .some(0){
    
}
//------------------------------

if let x = a{
    print(x)
}

if case .some(let x) = a{
    print(x)
}

if case let x? = a{
    print(x)
}


let list: [Int?] = [0, nil, nil, 3, nil, 5]

for item in list{
    guard let x = item else{ continue }//optional binding(guard)
    print(x)
}

for case let x? in list{
    print(x)
}
//코드가 간결해진다.

```

