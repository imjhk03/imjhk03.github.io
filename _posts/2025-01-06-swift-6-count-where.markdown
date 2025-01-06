---
title: "Swift 6의 새로운 기능: count(where:)"
layout: post
tags: [swift]
---

[SE-0220](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0220-count-where.md)에서 소개한 `count(where:)` 기능이 Swift 6에 추가 되면서 배열, Set 같은 곳에서 일치하는 항목의 개수를 구할 수 있다. 이전에는 `filter()`와 `count`를 조합해서 구할 수 있었는데 이는 즉시 버려지는 새로운 배열을 만드는 단점이 있었다. `count(where:)`는 이런 퍼포먼스 문제를 해결하고 사용자가 읽고 쉽게 사용할 수 있는 인터페이스를 제공한다.

```swift
let nums = [0,1,7,4,4,5]
let evenNumbers = nums.count(where: { $0.isMultiple(of: 2) } )
// evenNumbers == 3

let words = ["swift", "kotlin", "java", "python", "go"]
let longWords = words.count { $0.count > 4 }
// longWords == 3

let drinks = ["coffee", "juice", "water", "coffee", "juice", "coffee"]
let coffeeCount = drinks.count { $0 == "coffee" }
// coffeeCount = 3

struct User {
    let name: String
    let age: Int
}

let users = [
    User(name: "Kim", age: 25),
    User(name: "Lee", age: 30),
    User(name: "Park", age: 25)
]
let age25Count = users.count { $0.age == 25 }
// age25Count == 2
```

Sequnce 프로토콜을 채택한 곳에서 사용할 수 있으며, sequence는 유한해야 한다.

<br>
**참고**
<br>
[count\(where:\) | Apple Developer Documentation](https://developer.apple.com/documentation/swift/sequence/count%28where:%29)