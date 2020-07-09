## Optional Chaining

- Optional 의 결과는 항상 optional
- chaining중 하나라도 nil이라면 nil이다



```swift
struct Contacts {
    var email: [String: String]
    var address: String
}

struct Person{
    var name: String
    var contacts: Contacts
    
    init(name: String, email: String){
        self.name = name
        contacts = Contacts(email: ["home": email], address: "Seoul")
    }
}
//optional chaining
var p = Person(name: "DaeHyung", email: "eogud@example.com")
let a = p.contacts.address //optional chaining이 아님

var optionalP: Person? = Person(name: "DaeHyung", email: "eogud@example.com")

let b = optionalP?.contacts.address //optional chaining

optionalP = nil

let c = optionalP?.contacts.address


```

- 리턴되는 형식은 마지막 형식으로 결정된다.

- 위에 코드와 다르게 Contact를 Optional로 만들면 a 또한 optional chaining이 된다.

  - ```swift
    struct Person{
        var name: String
        var contacts: Contacts?
        
        init(name: String, email: String){
            self.name = name
            contacts = Contacts(email: ["home": email], address: "Seoul")
        }
    }
    
    var p = Person(name: "DaeHyung", email: "eogud@example.com")
    
    let a = p.contacts?.address //optional chaining이 되버림
    
    var optionalP: Person? = Person(name: "DaeHyung", email: "eogud@example.com")
    
    let b = optionalP?.contacts?.address //optional chaining
    
    optionalP = nil
    
    let c = optionalP?.contacts?.address    //optional chaining
    
    ```

- Contacts 구조체에 함수를 넣고 Person 구조체에 Optional 함수를 넣었을 때

  - ```swift
    struct Contacts {
        var email: [String: String]
        var address: String?
        
        func printAddress(){
            print(address ?? "no address")  //닐 콜레이싱
        }
    }
    
    struct Person{
        var name: String
        var contacts: Contacts?
        
        init(name: String, email: String){
            self.name = name
            contacts = Contacts(email: ["home": email], address: "Seoul")
        }
        func getContacts() -> Contacts? {
            return contacts
        }
    }
    
    var p = Person(name: "DaeHyung", email: "eogud@example.com")
    
    p.getContacts()?.address?.count //optional chaining
    
    let f: (() -> Contacts?)? = p.getContacts
    
    f?()?.address //물음표가 2개
    
    let d = p.getContacts()?.printAddress() // optional void
    
    if d != nil {
        
    }
    
    if let _ = p.getContacts()?.printAddress(){
        
    }
    
    ```

- Email 뒤에 ? 붙여서 Optional로 만들 때

  - ```swift
    
    
    struct Contacts {
        var email: [String: String]?
        var address: String?
        
        func printAddress(){
            print(address ?? "no address")  //닐 콜레이싱
        }
    }
    
    struct Person{
        var name: String
        var contacts: Contacts?
        
        init(name: String, email: String){
            self.name = name
            contacts = Contacts(email: ["home": email], address: "Seoul")
        }
        func getContacts() -> Contacts? {
            return contacts
        }
    }
    
    var p = Person(name: "DaeHyung", email: "eogud@example.com")
    
    
    let e = p.contacts?.email?["home"]
    
    p.contacts?.email?["home"]?.count
    
    
    p.contacts?.address = "Pankyo"
    p.contacts?.address
    
    var optionalP: Person? = nil
    optionalP?.contacts?.address = "Pankyo" //contact속성에 들어가지도 못함. nil
    optionalP?.contacts?.address
    
    
    
    ```

- 옵셔널 체이닝에서 가장 헷갈리는건 물음표를 어디에 붙이는지이다.

- Fix를 이용해서 물음표 위치를 도움받는다.



------------

##### 정리

- 옵셔널 체이닝의 결과는 항상 옵셔널
- 옵셔널 체이닝중  중간에 nil을 리턴한다면 전체 표현식을 보지 않고 nil을 리턴한다.

