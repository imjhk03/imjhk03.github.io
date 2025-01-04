---
title: Swift에서 숫자를 더 읽기 쉽게 작성하는 방법
layout: post
categories: [Tips]
tags: [swift]
---

스위프트에서 숫자를 더 읽기 쉽게 작성하는 방법이 있다. Int와 Float에서 모두 앞에 0을 추가할 수 있고 밑줄을 포함해서 가독성을 높일 수 있다. 이 방법들은 모두 기본 값에 영향을 미치지 않는다.

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1

let aNumber = 123.000
let bNumber = 000.456
```

<br>
**참고**
<br>
[Numeric Literals](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/thebasics/#Numeric-Literals){:target="_blank"}