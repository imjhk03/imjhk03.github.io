---
title: 문자열 배열을 하나의 문자열로 결합하는 방법
layout: post
categories: [Tips]
tags: [strings]
---

스위프트에서 문자열 배열을 하나의 문자열로 결합하는 간단한 메소드가 있습니다. 바로 **`joined()`**입니다.

```swift
let array = ["고양이", "강아지", "햄스터"]
let joined = array.joined(separator: ", ")
print(joined)
// Prints "고양이, 강아지, 햄스터"
```

**`separator`** 파라미터를 이용해서 합쳐지는 요소들 사이에 넣고 싶은 문자열을 추가할 수 있습니다.

[joined(separator:)](https://developer.apple.com/documentation/swift/array/3017516-joined)