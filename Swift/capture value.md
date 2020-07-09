## capturing value

- 캡쳐한다: 값을 가져와서 쓴다..
- 캡쳐하는 방법 2가지
  - 복사본을 캡쳐하는 방식 - Objective-C가 이방식
  - 참조를 캡쳐 - swift(원본을 그대로 가져옴, 값을 바꾸면 함께 바뀜)

```swift
var num = 0
let c = {
    num += 1    //참조이므로 체크포인트2에서도 1출력
    print("check point #1: \(num)")
}
c()
 
print("check point #2: \(num)")

```

