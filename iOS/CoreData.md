# Core Data

> 코어 데이터는 저장된 객체들의 일부분을 가져올 수 있으며, 어떤 객체를 변경한다면 그 파일의 해당 부분만 갱신할 수 있다.



- Entity
  - 테이블같은 개념.
- Attribute
  - 속성(이름, 나이, 성별)
- relationship
  - 여러개의 엔티티 관계.





## NSManageObject

> Core Data에 저장된 단일 객체를 나타낸다. 이를 사용하여 Core Data 영구 저장소에서 저장, 편집, 삭제한다.
>
> 정의한 속성 및 관계에 적합하게 데이터 모델의 엔티티 형식을 취할 수 있다.



## NSPersistentContainer

> 저장 및 코어 데이터에서 정보를 용이하게 검색하도록하는 Entity의 집합으로 구성되어 있다.
>
> Core Data 상태를 전체적으로 관리하는 객체, Data Model을 나타내는 객체 등이 있다.





## Core Data 코드 작동



```swift
//Core Data
func save(name: String){
  guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else{
    return
  }

  //1
  let managedContext = appDelegate.persistentContainer.viewContext
  //2
  let entity = NSEntityDescription.entity(forEntityName: "Person", in: managedContext)!

  let person = NSManagedObject(entity: entity, insertInto: managedContext)

  //3
  person.setValue(name, forKeyPath: "name")

  //4
  do{
    try managedContext.save()
    people.append(person)
  }catch let error as NSError{
    print("could not save. \(error), \(error.userInfo)")
  }
}
```

1. Core Data Store 에서 아이템을 저장하거나 검색하려면 **NSManagedObjectContext** 를 만져야한다.
   1. 새로운 managed object를 managed object context에 넣어야한다; 
   2. managed object context를 커밋을 변경하고 저장한다.
2. 새로운 객체를 만들고 managed object context에 넣어야한다. **NSManagedObject의 static 메서드인 entity(forEntityName:in:)** 로 할 수 있다. 
3. NSManagedObject, name 속성을 키-값 을 정확히 데이터 모델에 저장해야한다.
4. person을 변경하고 save를 호출한다.



여기까지 진행을 했는데도 데이터는 앱을 종료했을 때 사라질 것이다. 그 이유는 저장만하고 목록에 fetch 해주지 않았기 때문이다.

```swift
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        //1
        guard let appDelegate =  UIApplication.shared.delegate as? AppDelegate else{
            return
        }
        
        let managedContext = appDelegate.persistentContainer.viewContext
        
        //2
        let fetchRequest = NSFetchRequest<NSManagedObject>(entityName: "Person")
        
        //3
        do{
            people = try managedContext.fetch(fetchRequest)
        }catch let error as NSError{
            print("could  not fetch. \(error), \(error.userInfo)")
        }
    }

```



1. Core Data를 관리하려면 AppDelegate를 뽑아와서 거기 안에 있는 persistentContainer을 가져와야한다.(앞의 NSManagedObjectContext 같이)
2. 이름에서 알 수 있듯이 NSFetchRequest는 Core Data에서 가져오는 것을 담당하는 클래스다. Fetch요청을 하면 조건을 만족하는 일련의 객체를 fetch해올 수 있다.
3. Fetch 요청을 manage object context로 넘겨 무거운 작업을 한다. fetch request에 의해 조건에 맞는 object manage 객체의 배열을 반환한다.