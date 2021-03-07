---
title: Comparable enum
layout: post
author: Joohee Kim
description: From Swift 5.3 and later, enums can be comparable
tags: swift
---

From Swift 5.3 and later, enums can be comparable. We can compare two cases from the enum with ```>```, ```<``` and similar.

```swift
enum Sizes: Comparable {
    case S
    case M
    case L
}

let first = Sizes.S
let second = Sizes.L

print(first < second)
// will print true, S comes before L in the enum case list
```

Enums with associated values can also be comparable.

```swift
enum OrderStatus: Comparable {
    case purchased
    case readyToShip
    case shipping(progress: Int)
    case shippingComplete
}

let orderStatusList = [shippingComplete, readyToShip, shipping(progress: 0.5), shipping(progress: 0.3), purchased]
let sortedByStatus = orderStatusList.sorted()
print(sortedByStatus)
// purchased, readyToShip, shipping(progress: 0.3), shipping(progress: 0.5), shippingComplete
```

In the above example, array will be sorted similar with the enum case list. But ```shipping(progress: 0.5)``` will consider to be higher than ```shipping(progress: 0.3)```.


Refrence<br/>
[Synthesized Comparable conformance for enum types](https://github.com/apple/swift-evolution/blob/master/proposals/0266-synthesized-comparable-for-enumerations.md){:target="_blank"}
