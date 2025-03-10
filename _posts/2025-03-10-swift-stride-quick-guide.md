---
title: Swift Stride 빠른 가이드
layout: post
tags: [swift, sequences]
---

스위프트의 `stride()` 함수는 수열을 생성할 때 사용하는 강력한 도구다. 기본적인 for-loop에서 1씩 증가하는 대신, 원하는 크기만큼 증가하거나 감소하는 범위를 쉽게 만들 수 있다.

## stride() 함수 소개
Swift의 `stride()` 함수는 두 가지 형태가 있다:

1. `stride(from:to:by:)`: 끝값을 포함하지 않는 시퀀스를 생성한다.
   ```swift
   func stride<T>(
       from start: T,
       to end: T,
       by stride: T.Stride
   ) -> StrideTo<T> where T : Strideable
   
   // 시작값부터 끝값 직전까지 지정된 간격으로 반복
   for i in stride(from: 0, to: 10, by: 2) { print(i) }
   // Prints "0, 2, 4, 6, 8"
   ```
2. `stride(from:through:by:)`: 끝값을 포함하는 시퀀스를 생성한다.
   ```swift
   func stride<T>(
       from start: T,
       through end: T,
       by stride: T.Stride
   ) -> StrideThrough<T> where T : Strideable
   
   // 시작값부터 끝값까지 지정된 간격으로 반복
   for i in stride(from: 0, through: 10, by: 2) { print(i) }
   // Prints "0, 2, 4, 6, 8, 10"
   ```

끝값은 시작값에서 간격을 더했을 때 정확히 도달할 수 있는 값일 때만 포함된다:
```swift
// 두 예제는 동일한 결과를 출력한다
for i in stride(from: 0, to: 10, by: 3) { print(i) }
for i in stride(from: 0, through: 10, by: 3) { print(i) }
// Prints "0, 3, 6, 9"
```

음수 간격을 사용하여 감소하는 시퀀스를 만들 수 있다:
```swift
for i in stride(from: 10, through: 0, by: -2) { print(i) }
// Prints "10, 8, 6, 4, 2, 0"
```

만약 보폭 값이 시작 및 종료 값과 호환되지 않으면 빈 시퀀스가 표시된다.
```swift
for i in stride(from: 0, to: 10, by: -2) { print(i) }
// Prints nothing
```

## Strideable 프로토콜
`stride()` 함수는 `Strideable` 프로토콜을 준수하는 모든 타입에서 사용할 수 있다. 이 프로토콜은 다음과 같은 요구사항이 있다:

```swift
public protocol Strideable : Comparable {
    associatedtype Stride : SignedNumeric
    
    func distance(to other: Self) -> Stride
    func advanced(by n: Stride) -> Self
}
```

1. `Comparable` 준수 - 요소를 정렬할 수 있는지 확인한다.
2. `SignedNumeric`을 준수하는 연관된 `Stride` 타입 - 각 "단계"가 얼마나 클 수 있는지 정의한다.
3. 두 가지 필수 메서드:
   1. `distance(to:)` - 두 값 사이의 거리를 계산한다.
   2. `advanced(by:)` - 지정된 값만큼 이동한 새 값을 반환한다.

다음은 근무 시간을 나타내는 `Hour` 타입을 구현한 예제이다:
```swift
struct Hour: Strideable {
    var value: Int
    
    // Comparable 프로토콜 준수
    static func < (lhs: Hour, rhs: Hour) -> Bool {
        return lhs.value < rhs.value
    }
    
    static func == (lhs: Hour, rhs: Hour) -> Bool {
        return lhs.value == rhs.value
    }
    
    // Strideable 요구사항 구현
    typealias Stride = Int
    
    func distance(to other: Hour) -> Stride {
        return other.value - value
    }
    
    func advanced(by n: Stride) -> Hour {
        return Hour(value: value + n)
    }
}

// 사용 예시: 9시부터 17시까지 매 시간 체크
let workday = stride(from: Hour(value: 9), through: Hour(value: 17), by: 1)
for hour in workday {
    print("It's \(hour.value) o'clock")
}
```
이 예제는 근무 시간 스케줄링, 시간별 작업 계획 등에 활용할 수 있다.

<br>
**참고**
<br>
[stride\(from:to:by:\)](https://developer.apple.com/documentation/swift/stride%28from:to:by:%29){:target="_blank"}

[stride\(from:through:by:\)](https://developer.apple.com/documentation/swift/stride%28from:through:by:%29){:target="_blank"}

[Strideable](https://developer.apple.com/documentation/swift/strideable){:target="_blank"}

[Swift Stride Quick Guide](https://useyourloaf.com/blog/swift-stride-quick-guide/){:target="_blank"}

[\[Swift\] Strideable Protocol](https://min-i0212.tistory.com/39){:target="_blank"}