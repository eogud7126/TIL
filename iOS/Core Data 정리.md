## Core Data 동작



### 코어 데이터에 있는 것들을 어떻게 가져올까?

- NSManagedObjectContext가 필요하다.

  - 이것은 코어 데이터의 모든 활동이 이루어지는 허브 역할을 한다.

- 그렇다면 어떻게 context를 얻을까?

  - NSPersistentContainer로부터 받아올 수 있다.

  - ```swift 
    let container = (UIApplication.shared.delegate as! AppDelegate).persistentContainer
    ```

  - 매번 호출하기 번거롭기 때문에 AppDelegate에서 persistentContainer의 static 버전을 새로 만들어 쓰곤한다.

  - ```swift
    static var persistentContainer: NSPersistentContainer{
      return (UIApplication.shared.delegate as! AppDelegate).persistentContainer
    }
    
    ...사용할 때
    
    let coreDataContainer = AppDelegate.persistentContainer
    ```

- viewContext는 NSManagedObject중에서도 오직 메인스레드에서만 사용된다.(main Queue)

  - ```swift
    let context: NSManagedObjectContext = container.viewContext
    ```

  - 이 작업도 마찬가지로 static으로 빼주면 깔끔하다.

  - ```swift
    static var viewContext: NSManagedObjectContext {
      return persistentContainer.viewContext
    }
    .. 사용할 때
    
    let context = AppDelegate.viewContext
    ```

- NSEntityDescription 클래스

  - NSEntityDescription 클래스의 메소드는 NSManagedObject의 인스턴스를 리턴한다.
  - 데이터베이스의 모든 object들은 NSManagedObject 혹은 그것의 하위 클래스로 표현된다.

- 데이터를 insert하는 방법

  - ```swift
    let mail: NSManagedObject = NSEntityDescription.insertNewObject(forEntityName: "Mail", into: context)
    ```

- NSManagedObject 인스턴스 안의 속성에 접근하는 방법

  - ```swift
    let context = AppDelegate.viewContext
    if let mail = Mail(context: context) {
      mail.text = "asdasdasd"
      mail.created = Date()
    }
    ```

  - 

- context의 변경사항을 명시적인 save()를 통해 저장해야한다.

  - ```swift
    do{
      try context.save()
    }catch{
      //error handling
    }
    ```

- @NSManaged이 붙은 변수는 스위프트에게 NSManagedObject 의 슈퍼 클래스가 이 프로퍼티를 트겹ㄹ한 방법으로 조작할 것이라고 알리는 것이다.

- Deletion하는 방법

  - ```swift
    managedObjectContext.delete(_ object: tweet)
    ```

- NSFetchRequest

  - 데이터베이스에서 원하는 데이터를 캡슐화한다.

  - NSFetchRequest에서 요구되는 것

    - 어떤 Entity를 가져올지?
    - NSSortDescriptors 순서는 어떻게 할지?
    - NSPredicate 어떤 데이터를 원하는지?

  - ```swift
    //Create NSFetchRequest
    let request: NSFetchRequest<Mail> = Mail.fetchRequest()
    request.sortDescriptors = [sortDescriptor1, sortDescriptor2,..]
    request.predicate = ...
    
    //sortDescriptor
    let sortDescriptor = NSSortDescriptor(key: "name", ascending: true)
    
    //predicate
    let searchString = "foo"
    let predicate = NSPredicate(format: "name contains[c] %@", searchString)
    let dae = Mail(...)
    let predicate = NSPredicate(format: "created > %@ && name = %@", date, dae)
    ```