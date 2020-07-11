## TableView

<img src="/Users/user/Library/Application Support/typora-user-images/image-20200710103918155.png" alt="image-20200710103918155" style="zoom:50%;" />



- Table Header
  - 테이블 뷰에서 하나뿐임
  - 검색 버튼 생성 가능
- Table Footer
  - 테이블 가장 밑에 있는것
- Section(Optional)
  - 데이터 section
- Section Header 
  - 섹션의 가장 윗부분
- Table Cell
  - 데이터 row 0~~ row n



- Cell type 종류
  - subtitle
  - basic
  - Right detail
  - left detail



## UITableView Protocol

1. 섹션에 몇개를 넣을것인지
2. 각 섹션에 몇개의 Row를 넣을것인지
3. TableView로 UITableViewCell 중 하나를 가져오기. 하나의 Row를 채우기 위해 Cell에 요청



### Cell에서부터 Row 가져오기

```swift
func tableView(_ tv: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell{
  let data = myInternalDataStructure[indexPath.section][indexPath.row]
  let dequeued = tv.dequeueReusableCell(withIdentifier: "MyCell", for: indexPath)
 
  dequeued.textLabel?.text = data.importantInfo
  dequeued.detailTextLabel?.text = data.lessImportantInfo
  
  return cell
}
```



- dequeueReusableCell 메소드
  - UITableViewCell을 반환
  - 재사용

### UITableViewDataSource

- numberOfSections(in tv: UITableView) -> Int 
  - 섹션이 몇개인지 알게 해주는 메소드
  - 있어도 되고 없어도됨
  - default는 1
- numberOfRowsInSection
  - 반드시 필요한 메소드
  - 섹션 안에 몇개의 row가 있는지 알 수 있음
- cellForRowAt 
  - to return loaded-up UITableViewCells
- Dynamic prototype에서만 필요



## Table View Segues

- Preparing to segue from a row in a table view

```swift
func prepare(for segue: UIStoryboardSegue, sender: Any?){
  if let identifier = segue.identifier{
    switch identifier{
      case "XyzSegue": //handle here
      case "ABCSegue":
      	if let cell = sender as? MyTableViewCell,
      let indexPath = tableView.indexPath(for: cell),
      let seguedToMVC = segue.destination as? MyVC{
        seguedToMVC.publicAPI = data[indexPath.section][indexPath.row]
      }
      default: break
    }
  }
}
```



## UITableViewDelegate

- Delegate는 TableView를 어떻게 보여줄지

- Data Source는 실질적인 데이터

- > Model이 변경될 때?

  - func reloadData()
  - func reloadRows(at indexPath: , with animation: )