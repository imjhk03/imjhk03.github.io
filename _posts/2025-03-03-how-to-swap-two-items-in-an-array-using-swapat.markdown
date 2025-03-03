---
title: Swift 배열에서 swapAt()으로 두 항목 위치 바꾸기
layout: post
tags: [arrays]
---

스위프트에서 배열에 있는 두 항목을 바꿔주는 `swapAt()` 함수가 있다. 바꾸고 싶은 두 항목의 인덱스를 인자로 넣어서 사용하면 된다. 이 함수는 새로운 배열을 반환하지 않고 기존에 있는 배열을 수정해서 반환한다. 당연하겠지만 배열 넘어선 인덱스 값을 사용하면 에러가 발생한다.

```swift
var fruits = ["apple", "banana", "cherry", "date"]
fruits.swapAt(0, 2)
// fruits is now ["cherry", "banana", "apple", "date"]
```

> Time Complexity: O(1)
{: .prompt-info}

배열의 항목을 재정렬할 때 유용하지만 정렬 알고리즘 구현할 때도 도움이 된다.

```swift
func bubbleSort<T: Comparable>(_ array: inout [T]) {
    for i in 0..<array.count {
        for j in 1..<array.count - i {
            if array[j] < array[j-1] {
                array.swapAt(j, j-1)
            }
        }
    }
}

// 사용 예시
var numbers = [5, 2, 8, 1, 9]
bubbleSort(&numbers)
print(numbers) // [1, 2, 5, 8, 9]
```

위의 `bubbleSort` 함수는 버블 정렬 알고리즘을 사용하여 배열을 정렬한다. 배열의 각 요소를 반복하면서 인접한 요소와 비교하여 더 작은 값을 앞으로 이동시킨다. 이 과정에서 `swapAt()` 함수를 사용하여 두 요소의 위치를 바꾼다.

<br>
**참고**
<br>
[swapAt\(_:_:\) | Apple Developer Documentation](https://developer.apple.com/documentation/swift/array/swapat%28_:_:%29){:target="_blank"}