## Memo App

- Story Board의 테이블 뷰 셀의 Disclosure Indicator은 셀의 상세 화면이 있다고 > 표시를 해준다.

- AppDelegate에 데이터를 저장하는 이유

  - 뷰 컨트롤러는 생명주기가 짧기 때문에 화면을 전환할 때 이전 화면의 뷰 컨트롤러는 메모리에서 소멸된다. 
  - 그렇기 때문에 뷰 컨트롤러의 커스텀 클래스에는 데이터를 저장하는 것은 적합합지 않다.
  - AppDelegate는 전역변수를 저장하기에 적합하고, 앱 내에서 하나만 존재하고 어디서든지 접근할 수 있다.
  - 앱 자체의 생명주기와 운명을 함께하기 때문에 중간에 소멸되지 않는다.
  - 실수했던 점: var memolist = self.appDelegate.memolist를 하면 각각의 memolist가 생성되어 작동하지 않는다. 귀찮더라도 self.appDelegate.memolist 를 써줘야 한 리스트 안에 데이터가 들어감..

- 테이블 뷰에서 셀 선택시 넘어가는 화면 구현

  - ```swift
        override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
            //memolist에서 선택된 행에 맞는 데이터를 꺼낸다.
            let row = self.appDelegate.memolist[indexPath.row]
            
            //상세화면 인스턴스 생성: 중요!!
            guard let vc = self.storyboard?.instantiateViewController(withIdentifier: "MemoRead") as? MemoReadViewController else{
                return
            }
            
            //값을 전달한 다음 상세 화면으로 이동
            vc.param = row
            self.navigationController?.pushViewController(vc, animated: true)
        }
    ```

- Core Data 적용

  - MemoDAO 클래스를 만들어 메소드들을 정리해둔다.

    - ```swift
      //
      //  MemoDAO.swift
      //  Memo
      //
      //  Created by USER on 17/07/2020.
      //  Copyright © 2020 USER. All rights reserved.
      //
      
      import UIKit
      import CoreData
      
      class MemoDAO{
          lazy var context: NSManagedObjectContext = {
              let appDelegate = UIApplication.shared.delegate as! AppDelegate
              return appDelegate.persistentContainer.viewContext
          }()
          
          func fetch() -> [MemoData]{
              var memolist = [MemoData]()
              
              //요청 객체 생성
              let fetchRequest: NSFetchRequest<MemoMO> = MemoMO.fetchRequest()
              
              //최신 글 순으로 정렬
              let regdateDesc = NSSortDescriptor(key: "regdate", ascending: false)
              fetchRequest.sortDescriptors = [regdateDesc]
              
              do{
                  let resultset = try self.context.fetch(fetchRequest)
                  
                  //읽어온 결과 집합을 순회하면서 [MemoData] 타입으로 변환
                  for record in resultset{
                      let data = MemoData()
                      
                      //MemoMO 프로퍼티 값을 MemoData의 프로퍼티로 복사
                      data.title = record.title
                      data.contents = record.contents
                      data.regdate = record.regdate! as Date
                      data.objectID = record.objectID
                      
                      //이미지가 있을 때 복사
                      if let image = record.image as Data? {
                          data.image = UIImage(data: image)
                      }
                      
                      //MemoData 객체를 memolist 배열에 추가
                      memolist.append(data)
                  }
              }catch let error as NSError{
                  NSLog("An error has occured: %s", error.localizedDescription)
              }
              return memolist
          }
          
          func insert(_ data: MemoData){
              //관리 객체 인스턴스 생성
              let object = NSEntityDescription.insertNewObject(forEntityName: "Memo", into: self.context) as! MemoMO
              
              //MemoData로부터 값을 복사
              object.title = data.title
              object.contents = data.contents
              object.regdate = data.regdate
              
              if let image = data.image{
                  object.image = image.pngData()
              }
              
              //영구 저장소에 변경사항 반영
              do{
                  try self.context.save()
              }catch let error as NSError{
                  NSLog("An error has occurred: %s",error.localizedDescription)
              }
          }
          
          
          func delete(_ objectID: NSManagedObjectID) -> Bool {
              //삭제할 객체를 찾고 컨텍스트에서 삭제
              let object = self.context.object(with: objectID)
              self.context.delete(object)
              
              //영구 저장소에 변경 사항 적용
              do{
                  try self.context.save()
                  return true
              }catch let error as NSError{
                  NSLog("An error has occurred: %s", error.localizedDescription)
                  return false
              }
          }
      }
      
      ```




