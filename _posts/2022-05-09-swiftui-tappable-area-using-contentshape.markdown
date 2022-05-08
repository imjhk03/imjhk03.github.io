---
title: SwiftUI에서 contentShape()을 이용해서 뷰를 탭하게 하는 방법
layout: post
tags: [swiftui]
---

일반 `Text`나 `Image`을 사용하면 탭 제스처를 추가해서 탭 했을 때의 동작을 정의할 수 있다. 하지만 `VStack`이나 `HStack` 같은 container view에 제스처를 추가하면 생각처럼 잘 안될 때가 있다. 예를 들어, `HStack` 안에 `Image`와 `Text` 사이에 `Spacer`를 넣었다면, `Spacer` 영역을 탭 했을 때, 원하는 탭 제스처가 안 될 때가 있다.

이럴 때, `contentShape()`을 이용해서 탭 영역을 잡으면 가능하게 할 수 있다. 아래 코드와 같이 `HStack`에 `contentShape()` modifier를 추가하면 stack 전체를 탭 가능한 영역으로 만들 수 있다.


```swift
HStack {
    Text("Terms")
    Spacer()
    Image(systemName: "chevron.right")
}
.contentShape(Rectangle())
.onTapGesture {
    print("Show details for terms")
}
```