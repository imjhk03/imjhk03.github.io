---
title: Swift에서 짝수/홀수 확인하는 방법
description: Integer의 배수 확인하기
layout: post
tags: [swift]
---

[SE-0225](https://github.com/apple/swift-evolution/blob/master/proposals/0225-binaryinteger-iseven-isodd-ismultiple.md)에서 소개한 `isMultiple(of:)` 기능이 Swift 5에 추가 되면서 나누기 나머지 연산인 `%`를 사용하는 것보다 훨씬 더 명확한 방법으로 한 숫자가 다른 숫자의 배수인지 확인할 수 있다.

```swift
let randomNumber = 8

if randomNumber.isMultiple(of: 2) {
    print("Even")
} else {
    print("Odd")
}

// UITableView alternating row colour
cell.contentView.backgroundColor = indexPath.row % 2 == 0 ? .gray : .white
```

가장 많이 쓰이는 경우가 짝수인지 홀수인지 확인할 때인데, `randomNumber % 2 == 0` 으로도 확인할 수 있지만, 내재되어 있는 함수를 Xcode의 code completion으로 바로 찾아서 확인할 수 있는 이점이 있다.

<br>
**참고**
<br>
[SE-0225](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0225-binaryinteger-iseven-isodd-ismultiple.md){:target="_blank"}

[isMultiple(of:)](https://developer.apple.com/documentation/swift/int/ismultiple(of:)){:target="_blank"}