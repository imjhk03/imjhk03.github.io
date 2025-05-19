---
title: Swift 5.9의 if/switch 표현식으로 코드 간단하게 만들기
description: return 키워드 없이 더 간결해진 조건문 작성 방법
layout: post
categories: [Tips]
tags: [swift]
---

[SE-0380](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0380-if-switch-expressions.md){:target="_blank"}에서 `if`와 `switch` 표현식(expression)이 Swift 5.9에 추가 되면서 `return` 키워드 없이 코드를 더 줄여서 작성할 수 있게 되었다.

## switch 표현식 사용하기

```swift
enum Tab {
    case home, search, profile
    // Swift 5.9 이전
    var title: String {
        switch self {
        case .home: return "Home"
        case .search: return "Search"
        case .profile: return "My Profile"
        }
    }
    
    // Swift 5.9
    var title: String {
        switch self {
        case .home: "Home"
        case .search: "Search"
        case .profile: "My Profile"
        }
    }
}
```

위 예제에서 볼 수 있듯이, Swift 5.9부터는 `switch` 문에서 `return` 키워드를 생략할 수 있어 코드가 더 간결해졌다.

## if 표현식 사용하기

```swift
// 기존 방식
var score = 80
var result: String
if score >= 65 {
    result = "Pass"
} else {
    result = "Fail"
}

// Swift 5.9 - if 표현식
let result = if score >= 65 { "Pass" } else { "Fail" }

// Swift 5.9 - switch 표현식
let result = switch score {
    case 65...: "Pass"    // 범위 연산자 사용
    default: "Fail"
}
```