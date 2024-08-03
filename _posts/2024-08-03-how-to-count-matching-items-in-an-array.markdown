---
title: 배열에서 일치하는 항목을 계산하는 방법
layout: post
categories: [Tips]
tags: [arrays]
---

> Swift 6.0부터 `count(where:)` 메서드가 새로 나왔는데, 스위프트 6.0을 사용하지 않는다면 아래 확장해서 사용하면 된다.
{: .prompt-info }

스위프트에서 배열이나 컬렉션 안에서 특정 항목의 개수를 구하고 싶으면 `filter()`를 이용해서 구할 수 있다.

예를 들어, 0과 1을 구성하는 배열에서 1의 개수를 구하고 싶다면 아래와 같이 작성할 수 있다.
```swift
let allOnesCount1 = [0, 1, 0, 1, 1, 1, 0, 1].filter { $0 == 1 }.count
```

배열 같은 컬렉션에서 자주 사용할 수 있기 때문에 `Collection`을 확장해서 사용할 수 있다.
```swift
extension Collection {
    func count(where condition: (Element) throws -> Bool) rethrows -> Int {
        return try self.filter(condition).count
    }
}
```

이렇게 확장해서 사용한다면 아래와 같이 작성할 수 있다.
```swift
let allOnesCount2 = [0, 1, 0, 1, 1, 1, 0, 1].count { $0 == 1 }
```

<br>
**참고**
<br>

[How to count matching items in an array](https://www.hackingwithswift.com/example-code/language/how-to-count-matching-items-in-an-array)