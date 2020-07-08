## Access Control

- Swift의  access control은 module과 source file 단위
  - Module: code 배포의 단위(ex. App, Framework, Library)
  - Source file: 하나의 source file
- 다른 module의 code를 참조하려하면 import 시켜야함
- 같은 module 안에서는 다른 source file을 import할 필요 없음



### Access Levels

1. open
   - Module 외부에서 접근 가능
   - class에서만 사용, 정의된 모듈 밖에서도 상속 가능
2. public
   - module 외부에서 접근 가능
3. internal
   - 같은 module안에서 접근가능
   - access modifier를 지정하지 않으면 default로 사용됨
4. fileprivate
   - 같은 source file 안에서 접근 가능
5. private
   - 기능 정의 내부 및 동일한 파일 내의 extension에서만 접근 가능

- Unit Test에서 @testable attribute를 import 앞에 써주면 framework의 internal에 대해서도 접근 가능

```swift
open class SomeOpenClass(){}
public class sompPublicClass(){}
.
.
.
```

