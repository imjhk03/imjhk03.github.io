---
title: Swift에서 Set 관련 연산자 사용하기
description: Swift에서 집합 연산자를 사용하여 컬렉션의 교집합, 합집합, 차집합 등을 구현하는 방법
layout: post
categories: [Tips]
tags: [swift, collections, sets]
---

Set은 고유한 원소들을 다루고 여러 집합 간의 수학적 연산을 수행하는 데 강력한 Swift 컬렉션이다. Set을 이용해서 두 개의 집합 간의 관계를 파악할 수 있는 다양한 연산자를 제공한다.

```swift
let setA: Set<String> = ["사과", "바나나", "체리", "대추"]
let setB: Set<String> = ["체리", "대추", "엘더베리", "무화과"]
```

## 주요 Set 연산

### 1. 합집합 (Union)
두 집합의 모든 고유한 원소를 결합한다.

```swift
let combined = setA.union(setB)
// 결과: ["사과", "바나나", "체리", "대추", "엘더베리", "무화과"]

// 연산자 사용
let combined2 = setA.formUnion(setB)  // setA를 직접 수정
```

### 2. 교집합 (Intersection)
두 집합에 모두 존재하는 원소를 반환한다.

```swift
let common = setA.intersection(setB)
// 결과: ["체리", "대추"]

// 연산자 사용
let common2 = setA.formIntersection(setB)  // setA를 직접 수정
```

### 3. 대칭차집합 (Symmetric Difference)
둘 중 하나의 집합에만 있는 원소를 반환한다.

```swift
let exclusive = setA.symmetricDifference(setB)
// 결과: ["사과", "바나나", "엘더베리", "무화과"]
```

### 4. 차집합 (Subtracting)
첫 번째 집합에서 두 번째 집합에 존재하는 원소를 제거한다.

```swift
let difference = setA.subtracting(setB)  
// 결과: ["사과", "바나나"]
```

## 빠른 참조

| 연산 | 메서드 | 설명 | 수정 메서드 |
|------|--------|------|------------|
| 합집합 | `union(_:)` | 두 집합의 모든 원소 | `formUnion(_:)` |
| 교집합 | `intersection(_:)` | 공통 원소만 | `formIntersection(_:)` |
| 대칭차집합 | `symmetricDifference(_:)` | 각 집합에 고유한 원소 | `formSymmetricDifference(_:)` |
| 차집합 | `subtracting(_:)` | A에는 있지만 B에는 없는 원소 | `subtract(_:)` |

<br>
**참고**
<br>
[Set | Apple Developer Documentation](https://developer.apple.com/documentation/swift/set){:target="_blank"}