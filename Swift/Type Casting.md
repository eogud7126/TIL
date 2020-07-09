## Type Casting

- Instance의 type을 check하기 위한 방법

- class 상속 구조 속에서 instance를 superclass 혹은 subclass의 instance로 다루기 위한 방법

- is와 as operator 사용

  - is 는 어떤 class type인지 확인하기 위해 사용

    - ```swift
      var movieCount = 0
      var songCount = 0
      for item in library{
        if item is Movie{
          movieCount += 1
        }else if item is Song{
          songCount += 1
        }
      }
      ```



#### Downcasting

- subclass type 으로 downcast 해야하는 경우 as? 또는 as!를 사용

- downcast는 실패할 수 있기 때문에 as?를 사용하면 optional value를 받아올 수 있고 as!를 사용하면 force unwrapping됨.

- ```swift
  for item in library{
    if let movie = item as? Movie {
      print("Movie: \(movie.name),dir.\(movie.director)")
    }else if let song = item as? Song{
      print("Song: \(song.name), by \(song.artist)")
    }
  }
  ```



#### Any, AnyObject

- Swift에는 type에 상관없이 값을 다루기 위한 두개의 특별한 type alias가 있음.
  - AnyObject: 어떤 class type이라도 될수 있다.
  - Any: 어떤 type이라도 될 수 있다. Function type 포함.



#### Extension

- 기존의 class, struct, enum을 확장하여 새로운 기능을 더할 수 있게 해줌

- 원본 소스에 접근할 수 없어도 사용가능

- ```swift
  extension Int{
      func repetitions(task: () -> Void){
          for _ in 1...self {
              task()
          }
      }
  }
  
  3.repetitions {
      print("Hello!")
  }
  
  //Hello! 3번
  ```

