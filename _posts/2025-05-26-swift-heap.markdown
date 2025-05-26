---
title: Swift Collections의 Heap 사용하기
description: Swift Collections 패키지의 Heap 자료구조 활용 방법
layout: post
categories: [Tips]
tags: [swift, collections]
---

Heap은 우선순위 큐를 구현할 때 유용한 자료구조이다. Swift Collections 패키지를 추가하면 쉽게 사용할 수 있다.

## 패키지 추가
```swift
dependencies: [
    .package(url: "https://github.com/apple/swift-collections.git", from: "1.0.0")
]
```

## 기본 사용법
```swift
import Collections

// 초기화
var heap = Heap<Int>()

// 삽입
heap.insert(10)
heap.insert(20)

// 조회
let min = heap.min  // 최소값
let max = heap.max  // 최대값

// 삭제
while let min = heap.popMin() {
    print("다음 최소값:", min)
}

while let max = heap.popMax() {
    print("다음 최대값:", max)
}
```

## 커스텀 타입 사용하기
```swift
// Comparable 프로토콜 준수 필요
struct Task: Comparable {
    let name: String
    let priority: Int
    
    // 우선순위 비교 구현
    static func < (lhs: Task, rhs: Task) -> Bool {
        lhs.priority < rhs.priority
    }
}

// 우선순위 큐 구현 예시
var taskQueue = Heap<Task>()

// 작업 추가
taskQueue.insert(Task(name: "긴급 작업", priority: 10))
taskQueue.insert(Task(name: "일반 작업", priority: 5))
taskQueue.insert(Task(name: "낮은 우선순위 작업", priority: 1))

// 우선순위가 높은 작업부터 처리
while let task = taskQueue.popMax() {
    print("처리중: \(task.name) (우선순위: \(task.priority))")
}
```

>Heap에 저장되는 타입은 반드시 `Comparable` 프로토콜을 준수해야 한다.
{: .prompt-warning}

<br>
**참고**
<br>

[Heap Documentation](https://github.com/apple/swift-collections/blob/main/Documentation/Heap.md){:target="_blank"}
