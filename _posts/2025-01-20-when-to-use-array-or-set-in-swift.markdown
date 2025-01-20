---
title: "Swift의 Array와 Set: 언제 어떤 컬렉션을 선택할까?"
layout: post
tags: [arrays, sets]
---

Swift에서 컬렉션(collection)으로 작업할 때는 배열(array)과 집합(set)이라는 두 가지 기본 데이터 구조가 자주 등장한다. 어떤 경우에서 적절한 데이터 구조를 사용할지에 대해 자세히 알아보자.

공식 문서에는 배열과 집합을 아래와 같이 정의한다.
* 배열(Array): 정렬된 무작위 액세스 컬렉션(*“An ordered, random-access collection.”*)
* 집합(Set): 정렬되지 않은 고유 요소의 컬렉션(*“An unordered collection of unique elements.”*)

## 기본
배열은 정수부터 문자열, 클래스까지 모든 종류의 항목(element)을 저장할 수 있다. 반면에 집합은 `Hashable` 프로토콜을 준수하는 모든 항목 타입으로 만들 수 있다. 기본적으로 문자열, 숫자 및 부울 타입, 연관 값(associated value)이 없는 열거형(enum), 심지어 집합 자체를 포함하여 표준 라이브러리의 대부분의 타입은 `Hashable` 이다.

배열과 집합은 초기화할 때 작성하는 코드 보면 서로 비슷하다. 집합과 다르게 배열은 조금 더 짧게 만들 수 있다.
```swift
var favoriteColors: Array<String> = ["blue", "red", "blue", "green"]
let anotherFavoriteColors: [String] = ["blue", "red", "blue", "green"]
print(favoriteColors)
// Prints: ["blue", "red", "blue", "green"]

var uniqueColors: Set<String> = ["blue", "red", "blue", "green"]
print(uniqueColors)
// Prints: ["red", "green", "blue"]
```

## 순서의 차이
배열은 순서가 보장되기 때문에 인덱스를 가지고 배열의 항목을 접근할 수 있다. 단, 배열의 길이 보다 넘어서는 곳에 접근하지 않도록 주의해야 한다.
```swift
print(anotherFavoriteColors[1])
// Prints: "red"
```

하지만 집합은 순서가 보장되지 않기 때문에 원하는 값을 바로 접근할 수 없다. 대신 값이 집합에 있는지 확인할 수 있다.
```swift
if uniqueColors.contains("red") {
    print("I like red.")
} else {
    print("I don't like red")
}
```

배열은 순서가 있기 때문에 첫번째 항목, 마지막 항목을 구할 수 있는 메서드가 제공된다.
```swift
if let firstElement = favoriteColors.first, let lastElement = favoriteColors.last {
    print(firstElement, lastElement, separator: ", ")
}
// Prints: "blue, green"
```

집합도 첫번째 항목을 구할 수 있지만, 순서가 보장되지 않기 때문에 호출할 때 마다 값이 매번 다르게 나온다.
```swift
if let firstElement = uniqueColors.first {
    print(firstElement, separator: ", ")
}
// Prints: "blue" or "red" or "green"
```

## 중복의 차이
배열은 중복을 허용하는 것과 달리 집합은 중복을 허용하지 않는다. 그래서 중복이 없는 서로 다른 값들만 가지고 있어야 할 때 집합을 사용하는 것이 더 좋다.

배열과 집합에 항목을 추가할 때 사용하는 메서드에도 차이가 있다. 배열은 `append(_:)` 를 사용해서 마지막에 항목을 추가하는데, 집합의 `insert(_:)`는 `inserted` 부울 값과 이미 존재하는 항목 또는 방급 삽입된 항목을 포함하는 `memberAfterInsert` 프로퍼티를 반환한다. 이는 추가하려는 항목이 있는지 확인할 수 있다.
```swift
favoriteColors.append("red")
print(favoriteColors)
// Prints: ["blue", "red", "blue", "green", "red"]

let (inserted, memberAfterInsert) = uniqueColors.insert("red")
if !inserted {
    print("\(memberAfterInsert) already exists")
}
// Prints: red already exists
```

## 퍼포먼스의 차이
배열과 집합의 특징 때문에 어떤 작업을 할 때, 어떤 데이터 구조를 선택하느냐에 따라 퍼포먼스가 다르게 나올 수 있다. 배열은 인덱스를 통해 항목을 접근할 때 (O(1)) 유용하고, 집합은 항목이 존재하는지 확인할 때 (O(1)) 유용하다. 집합은 순서를 가지고 중복 데이터를 다룰 때 적합하고, 집합은 중복 데이터 없는 고유의 값들만 가지고 데이터를 다룰 때 적합하다. 또한 집합은 다른 집합과의 교집합을 확인하는 등의 수학적 집합 연산을 제공해서 상황에 따라 유용하게 사용할 수 있다.

## 결론
배열과 집합은 서로 비슷하지만 확연한 차이점이 있다. 정리하면 아래와 같다.

배열을 선택하는 경우:
* 순서가 중요한 경우
* 위치별로 항목에 접근해야 하는 경우
* 중복이 의미 있는 경우(예: 발생 횟수 계산)

집합을 선택하는 경우:
* 고유성을 보장해야 하는 경우
* 순서는 중요하지 않음
* 항목의 존재를 자주 확인하는 경우
* 집합 연산을 수행해야 하는 경우


<br>
**참고**
<br>

[Array](https://developer.apple.com/documentation/swift/array){:target="_blank"}

[Set](https://developer.apple.com/documentation/swift/set){:target="_blank"}

[Array vs Set: Fundamentals in Swift explained](https://www.avanderlee.com/swift/array-vs-set-differences-explained/#performance-differences){:target="_blank"}