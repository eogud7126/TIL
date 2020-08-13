### Core Data에서 Boolean 자료형 Predicate 이슈



- 필터링을 구현하는 도중 읽은 메일과 읽지 않은 메일을 구별하는 필터링을 적용하기 위해 boolean 자료형을 predicate 했습니다.

```swift
predicates.append(NSPredicate(format: "isRead == %@", isRead))
```

- 당연히 필터링이 될 줄 알았지만, 이슈가 있었습니다.

<img src="./boolean predicate issue.png" alt="issue" style="zoom:100%;" />

- 앱 크래시가 일어났고 Predicate 문서에서 해당 원인을 찾아보니

  ```
  %@ is a var arg substitution for an object value—often a string, number, or date.
  ```

  %@는 문자열, 숫자, 날짜에 대한 var arg라고 나와있었습니다.

- Bool 변수를 NSNumber을 통해 숫자로 바꾸니 정상적으로 작동되는 것을 확인했습니다.

```swift
predicates.append(NSPredicate(format: "isRead == %@", NSNumber(value: isRead)))
```

