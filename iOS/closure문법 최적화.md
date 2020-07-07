## Swift 문법 최적화 (Syntax Optimization)



- Swift는 가장 간소화된 것을 좋아한다.

- 정식 문법과 단축 문법을 비교해보면 같은 기능을 하는 코드인지 모를 때도 있다.

- ```swift
  import Foundation
  
  let products = ["iPone Xs","iPhone Xr", "iPhone 8", "iPhone 7",
                  "AirPods", "Apple Watch Series 4", "Apple Watch Nike+",
                  "MacBook Air", "MacBook Pro", "iMac", "iMac Pro", "Mac Pro",
                  "Mad mini"]
  
  
  var proModels = products.filter({ (name: String) -> Bool in
      return name.contains("Pro")
      
  })
  
  //단축화
  products.filter {
      $0.contains("Pro")
  }
  
  proModels.sort( by: { (lhs: String, rhs: String) -> Bool in
      return lhs.caseInsensitiveCompare(rhs) == .orderedAscending
  })
  
  //1단계 단축: 파라미터 형식과 리턴형을 생략한다.
  proModels.sort(by: { (lhs, rhs) in
      return lhs.caseInsensitiveCompare(rhs) == .orderedAscending
  })
  //2단계 단축: 파라미터 이름을 생략하고 shortened argument name으로 생략한다
  proModels.sort(by: {
      return $0.caseInsensitiveCompare($1) == .orderedAscending
  })
  //3단계 단축: 클로져에 포함된 것이 단일 리턴문이라면 리턴 키워드를 생략한다.
  proModels.sort(by: {
      $0.caseInsensitiveCompare($1) == .orderedAscending
  })
  //4단계 단축: 클로저가 마지막 파라미터라면 트레일링 클로져로 작성한다.
  proModels.sort(){
      $0.caseInsensitiveCompare($1) == .orderedAscending
  }
  //5단계 단축: 괄호 사이에 파라미터가 없다면 괄호를 생략한다.
  proModels.sort{
      $0.caseInsensitiveCompare($1) == .orderedAscending
  }
  
  ```

