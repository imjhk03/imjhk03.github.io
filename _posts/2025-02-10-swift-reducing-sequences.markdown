---
title: Swift 시퀀스 줄여서 값 도출하기 - reduce
layout: post
categories: [Tips]
tags: [swift, sequences]
---

스위프트 고차 함수들 중에 `map`과 함께 `reduce` 함수를 많이 사용하게 된다. 시퀀스를 클로저를 통해 하나의 값으로 도출하는 강력한 함수이다.
```swift
let numbers = [1, 2, 3, 4, 5]
let sum = numbers.reduce(0, +)
// or let sum = numbers.reduce(0) { $0 + $1 }
print(sum) // 15
```

`reduce`와 비슷한 `reduce(into:)` 함수를 이용해서 새로운 데이터 모델로도 만들 수 있다.
```swift
let str = "attack"

// Not using reduce(into:)
var frequencyMap = [Character: Int]()
for char in str {
    frequencyMap[char, default: 0] += 1
}

// Using reduce(into:)
// 축약형
let frequencyMap2 = str.reduce(into: [Character: Int]()) { $0[$1, default: 0] += 1 }

// 파라미터를 명시적으로 표현한 형태
let frequencyMap3 = str.reduce(into: [Character: Int]()) { dict, char in 
    dict[char, default: 0] += 1 
}
```