## 구조체 Mutating

- 구조체의 메서드가 구조체 내부에서 데이터를 수정할 때 mutating 키워드를 선언해줘야함

- ```swift
  struct Point{
    var x = 0
    var y = 0
    func moveTo(x: Int, y: Int){
      self.x = x	//컴파일 에러
      self.y = y	//컴파일 에러
    }
  }
  -> 컴파일 오류가 발생하지 않게 하기 위해 func키워드 앞에 mutating작성
  ```