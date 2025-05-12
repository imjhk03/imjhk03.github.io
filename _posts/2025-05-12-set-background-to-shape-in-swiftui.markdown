---
title: SwiftUI에서 도형을 배경으로 설정하기
description: background(_:in:fillStyle:) modifier를 사용한 배경 스타일링
layout: post
categories: [Tips]
tags: [swiftui]
image:
    path: /assets/img/2025/05/12/image1.png
---

SwiftUI에서 View의 배경을 캡슐 모양이나 직사각형 모양으로 만들고 싶을 때, `background(_:in:fillStyle:)` 사용하면 된다.

## 기본 사용법

```swift
Text("Hello, world!")
    .foregroundStyle(.white)
    .padding()		// padding을 써서 여유 있는 모양으로 만들 수 있음
    .background(
        Color.blue.gradient,
        in: Capsule()
)
```

![캡슐 모양의 파란색 그라데이션 배경이 있는 텍스트](/assets/img/2025/05/12/image1.png)

## 사용 가능한 도형들

이 modifier를 사용하여 다음과 같은 [`InsettableShape`](https://developer.apple.com/documentation/swiftui/insettableshape){:target="_blank"} 프로토콜을 준수하는 도형들을 뷰 뒤에 레이어링할 수 있다:

- [`Rectangle`](https://developer.apple.com/documentation/swiftui/rectangle){:target="_blank"} - 직사각형 모양
- [`Circle`](https://developer.apple.com/documentation/swiftui/circle){:target="_blank"} - 원형 모양
- [`Capsule`](https://developer.apple.com/documentation/swiftui/capsule){:target="_blank"} - 캡슐 모양
- [`RoundedRectangle`](https://developer.apple.com/documentation/swiftui/roundedrectangle){:target="_blank"} - 둥근 모서리 직사각형

## 다양한 스타일 예시

```swift
VStack(spacing: 10) {
    // 기본 색상 배경
    Text("Rectangle")
        .padding()
        .background(.red.gradient, in: Rectangle())

    // 둥근 모서리 직사각형
    Text("Rounded Rectangle")
        .padding()
        .background(.green.gradient, in: RoundedRectangle(cornerRadius: 12))

    // 그라데이션 배경
    Text("Capsule")
        .padding()
        .background(.blue.gradient, in: Capsule())
}
.foregroundStyle(.white)
```

![다양한 도형과 그라데이션이 적용된 텍스트 예시](/assets/img/2025/05/12/image2.png)

>다른 뷰와 함께 배경을 지정하거나 화면 전체 백그라운드 만드는 방법은 이 [글](https://imjhk03.github.io/posts/swiftui-add-background/)에서 참고하면 된다.
{: .prompt-tip}

<br>
**참고**
<br>

[background\(_:in:fillStyle:\)](https://developer.apple.com/documentation/swiftui/view/background(in:fillstyle:)-96bda){:target="_blank"}
