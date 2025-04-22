---
title: SwiftUI Color를 gradient으로 변경하기
description: iOS 16의 새로운 Color.gradient 프로퍼티 활용하기
layout: post
categories: [Tips]
tags: [swiftui]
---

>iOS 16.0 이상부터 사용할 수 있다.
{: .prompt-info}

SwiftUI에서 색상에 gradient를 쉽게 입힐 수 있다. iOS 16부터 SwiftUI `Color`의 `.gradient` 프로퍼티를 호출하기만 하면 해당 색상이 자동으로 멋진 그라데이션으로 바뀐다:

```swift
VStack {
    Rectangle()
        .fill(.blue)     // 일반 색상
    
    Rectangle()
        .fill(.blue.gradient)    // 일반 색상에 gradient 추가
}
.padding()
```

![파란색 사각형과 그라데이션이 적용된 사각형](/assets/img/2025/04/21/image1.png)

<br>
**참고**
<br>
[gradient | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/color/gradient){:target="_blank"}