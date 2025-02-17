---
title: "Swift의 isEmpty와 count == 0: 무엇이 다를까?"
layout: post
tags: [swift, collections]
---

스위프트에서 컬렉션(collection)이 비어 있는지 확인하는 방법이 두 가지가 있다. 컬렉션의 `count` 값이 `0` 이거나 `isEmpty` 프로퍼티를 사용한다. 이 둘의 차이점이 있는지 알아보자.

## isEmpty
Swift standard library에서 `isEmpty`가 어떻게 구현되어 있는지 보면 아래와 같다. `isEmpty`는 `count == 0` 으로 확인하지 않고 `startIndex` 와 `endIndex` 값이 같은지 확인한다. 이는 `Collection` 프로토콜의 기본 구현으로, `Set`과 같은 특별한 경우를 제외하고는 대부분의 컬렉션 타입들이 이 구현을 상속받아 사용한다.
```swift
extension Collection {
  /// A Boolean value indicating whether the collection is empty.
  ///
  /// When you need to check whether your collection is empty, use the
  /// `isEmpty` property instead of checking that the `count` property is
  /// equal to zero. For collections that don't conform to
  /// `RandomAccessCollection`, accessing the `count` property iterates
  /// through the elements of the collection.
  ///
  ///     let horseName = "Silver"
  ///     if horseName.isEmpty {
  ///         print("My horse has no name.")
  ///     } else {
  ///         print("Hi ho, \(horseName)!")
  ///     }
  ///     // Prints "Hi ho, Silver!")
  ///
  /// - Complexity: O(1)
  @inlinable
  public var isEmpty: Bool {
    return startIndex == endIndex
  }
}
```

## count == 0
반면에 `count` 구현하는 부분을 보면 `startIndex`에서 `endIndex`까지 거리를 계산한다. `Array`나 `Dictionary`와 같은 기본 컬렉션 타입들은 내부적으로 요소의 개수를 추적하고 있어 `O(1)`의 시간 복잡도를 가지지만, 모든 컬렉션 타입이 그런 것은 아니다.
```swift
extension Collection {
  /// The number of elements in the collection.
  ///
  /// To check whether a collection is empty, use its `isEmpty` property
  /// instead of comparing `count` to zero. Unless the collection guarantees
  /// random-access performance, calculating `count` can be an O(*n*)
  /// operation.
  ///
  /// - Complexity: O(1) if the collection conforms to
  ///   `RandomAccessCollection`; otherwise, O(*n*), where *n* is the length
  ///   of the collection.
  @inlinable
  public var count: Int {
    return distance(from: startIndex, to: endIndex)
  }

  /// Returns the distance between two indices.
  ///
  /// Unless the collection conforms to the `BidirectionalCollection` protocol,
  /// `start` must be less than or equal to `end`.
  ///
  /// - Parameters:
  ///   - start: A valid index of the collection.
  ///   - end: Another valid index of the collection. If `end` is equal to
  ///     `start`, the result is zero.
  /// - Returns: The distance between `start` and `end`. The result can be
  ///   negative only if the collection conforms to the
  ///   `BidirectionalCollection` protocol.
  ///
  /// - Complexity: O(1) if the collection conforms to
  ///   `RandomAccessCollection`; otherwise, O(*k*), where *k* is the
  ///   resulting distance.
  @inlinable
  public func distance(from start: Index, to end: Index) -> Int {
    _precondition(start <= end,
      "Only BidirectionalCollections can have end come before start")

    var start = start
    var count = 0
    while start != end {
      count = count + 1
      formIndex(after: &start)
    }
    return count
  }
}
```

여기서 더 자세히 보면 아래와 같이 설명되어 있다.
> O(1) if the collection conforms to RandomAccessCollection; otherwise, O(n), where n is the length of the collection.

즉 `RandomAccessCollection`을 채택하고 있다면 시간 복잡도가 `O(1)`이 되고, 그렇지 않다면 컬렉션의 길이 `n` 만큼 시간이 걸린다. 예를 들어, `Array`와 `Dictionary`는 `RandomAccessCollection`을 채택하고 있어 `O(1)`의 시간 복잡도를 가지지만, `Set`은 이를 채택하고 있지 않다. 그래서 `Set`의 `isEmpty` 구현을 보면 다음과 같이 `count` 값을 확인하는 방식을 사용한다:
```swift
extension Set: Collection {
  /// A Boolean value that indicates whether the set is empty.
  @inlinable
  public var isEmpty: Bool {
    return count == 0
  }
}
```

## Strings
컬렉션 중에서도 문자열을 다룰 때에도 `isEmpty` 혹은 `count`를 사용하는 경우가 많다. 스위프트 문자열은 복잡한 문자 모음으로, 여러 기호가 결합이 되어 하나의 문자로 표시될 수 있다. Swift의 String은 내부적으로 Unicode scalar values를 사용하며, 필요할 때 UTF-8로 인코딩된다. 이로 인해 보여지는 하나의 문자가 사실 여러 값을 포함하고 있을 수 있다. 그래서 문자열의 정확한 문자 수를 알려면 실제로 시작 인덱스와 끝 인덱스 사이의 정확한 거리를 계산하기 위해 모든 유니코드 스칼라 값들을 반복(`O(n)` 연산)해야 한다.
```swift
extension String: BidirectionalCollection {
  /// The number of characters in a string.
  ///
  /// To check whether a string is empty,
  /// use its `isEmpty` property instead of comparing `count` to zero.
  ///
  /// - Complexity: O(n), where n is the length of the string.
  @inline(__always)
  public var count: Int {
    return distance(from: startIndex, to: endIndex)
  }
}
```

이 두 인덱스 프로퍼티에 접근하는 데 실제 계산이 필요하지 않기 때문에 `Collection` 프로토콜의 기본 구현인 `isEmpty`는 `count`를 사용하는 대신 `startIndex`와 `endIndex` 프로퍼티가 동일한지 여부를 확인한다.

실제로 성능 차이를 보여주는 간단한 예제를 살펴보면:
```swift
let largeString = String(repeating: "a", count: 1000000)

// O(1) 연산 - 즉시 결과 반환
let isEmptyCheck = largeString.isEmpty

// O(n) 연산 - 전체 문자열을 순회해야 함
let countCheck = largeString.count == 0
```

## 마무리
컬렉션이 비어 있는지 여부를 확인할 때는 `count` 보다 빠르고 가독성이 좋은 `isEmpty`를 사용하도록 하는 것이 더 좋다. 만약 특정 개수를 확인하고 싶다면 그때는 `count`를 쓰는 것이 좋다. 특히 String이나 커스텀 컬렉션을 다룰 때는 이 차이가 성능에 큰 영향을 미칠 수 있다는 점을 기억하자.

<br>
**참고**
<br>
[Swift Collection.swift 소스코드](https://github.com/swiftlang/swift/blob/main/stdlib/public/core/Collection.swift#L811){:target="_blank"}

[Swift Set.swift 소스코드](https://github.com/swiftlang/swift/blob/main/stdlib/public/core/Set.swift){:target="_blank"}

[Swift StringCharacterView.swift 소스코드](https://github.com/swiftlang/swift/blob/main/stdlib/public/core/StringCharacterView.swift){:target="_blank"}

[Swift by Sundell: Using count vs isEmpty](https://www.swiftbysundell.com/articles/count-vs-isEmpty/){:target="_blank"}