---
title: 스위프트에서 문자열 비교하는 방법
layout: post
tags: [swift, strings]
---

스위프트(swift)에서 문자열을 비교하는 여러 방법들이 있다. 각각의 방법에 대해 자세히 알아보자.

## 일반적인 방법
스위프트에서 문자열 비교를 다룰 때 보통 ‘==’ 연산자 사용해서 비교한다. 이는 문자(Character)를 비교할 때도 동일하다.

```swift
let password = "password"
let passwordConfirmation = "password"

// Basic equality check
password == passwordConfirmation
// true
```

이 방법은 가장 간단하고 직관적인 방법으로, 두 문자열이 정확히 같은지 확인할 때 사용한다.

## 대소문자를 무시하고 문자열 비교하기
대소문자를 무시하고 비교하고 싶다면 `lowercased()` 혹은 `uppercased()`를 가지고 비교하는 경우가 있다.

```swift
let char = "a"
let capitalChar = "A"

char.lowercased() == capitalChar.lowercased()
// true
char.uppercased() == capitalChar.uppercased()
// true
```

하지만 이 방법보다 더 나은 방법이 있다. 우선 `lowercased()` 같은 함수는 사용할 때 마다 새로운 문자열을 복사하기 때문에 성능에 영향을 끼칠 수 있다. 그리고 언어와 지역에 따라 규칙이 다르게 적용이 될 수 있기 때문에 조금 더 안전한 방법을 써야한다.

아래는 문자열에 악센트가 있을 경우에 올바른 결과가 나타나지 않는 것을 확인할 수 있다.

```swift
let word = "pokemon"
let anotherWord = "Pokémon"

word.lowercased() == anotherWord.lowercased()
// false
```

## Compare 함수 사용하기
스위프트에서 제공하는 `compare(_:options:)` 함수를 사용해서 대소문자 무시하고 비교할 수 있다. 축약형으로 `caseInsensitiveCompare(_:)`를 쓸 수도 있다. 이 함수는 `ComparisonResult` 타입을 반환하며 다음과 같은 값들이 있다:
- `.orderedSame`: 두 문자열이 동일할 때
- `.orderedAscending`: 첫 번째 문자열이 두 번째 문자열보다 앞설 때
- `.orderedDescending`: 첫 번째 문자열이 두 번째 문자열보다 뒤설 때

```swift
let char = "a"
let capitalChar = "A"

let comparisonResult = char.compare(capitalChar, options: .caseInsensitive)

if comparisonResult == .orderedSame {
    // a is equals to A
}

// shortform
let shortComparisonResult = char.caseInsensitiveCompare(capitalChar)
```

악센트 기호는 일부 언어에서 사용하는 문자 또는 기본 글리프에 추가되는 글리프다. 이런 악센트 기호가 있는 경우를 무시하고 싶다면 `compare(_:options:)`에서 옵션으로 `.diacriticInsensitive`를 추가한다.

```swift
let word = "pokemon"
let anotherWord = "Pokémon"

let comparisonResult = word.compare(
    anotherWord,
    options: [.caseInsensitive, .diacriticInsensitive]
)

if comparisonResult == .orderedSame {
    print("Gotta catch 'em all!")
}
```

## 지역화된 비교
특정 언어나 지역의 규칙을 따라 문자열을 비교하고 싶다면 `.localized` 옵션을 사용할 수 있다:

```swift
let result = "café".compare(
    "cafe",
    options: [.localized, .caseInsensitive]
)
```

<br>
**참고**
<br>
[compare(_:options:)](https://developer.apple.com/documentation/foundation/nsstring/1408732-compare){:target="_blank"}

[Different ways to compare string in Swift](https://sarunw.com/posts/different-ways-to-compare-string-in-swift/){:target="_blank"}
