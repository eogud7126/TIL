## cleanup action

- 특정 code block을 현재 code block을 벗어날 때 반드시 불리게 할 수 있음
- error가 발생시 code 실행 위치가 jump해서 code block을 빠져나가게 되는데 이 때 빠져나가기 전에 반드시 처리해야 할 code를 실행하게 할 수 있음
- Defer 구문을 이용해서 작성된 code block은 현재 code block을 벗어날 때 불리게 되어 있음

```swift
func processFile(filename: String) throws{
    if exists(filename){
        let file = open(filename)
        defer{
            close(file) // 코드블록을 벗어날 때 반드시 호출됨
        }
        while let line = try file.readline() {
            //Work with file.
        }
        //close(file) is called here, at the end of the scope
    }
    
}
```

 