---
title: HierarchicalShapeStyle로 색상 계층 구조 활용하기
description: SwiftUI에서 계층적 색상 스타일 활용하기
layout: post
categories: [Tips]
tags: [swiftui]
---

SwiftUI에서 아주 간단하게 계층적으로 색상을 표현할 수 있다. `HierarchicalShapeStyle`을 사용하면 다섯 단계의 색상 계층 구조를 쉽게 구현할 수 있다.

```swift
VStack {
    RoundedRectangle(cornerRadius: 16)
        .fill()
    RoundedRectangle(cornerRadius: 16)
        .fill(.secondary)
    RoundedRectangle(cornerRadius: 16)
        .fill(.tertiary)
    RoundedRectangle(cornerRadius: 16)
        .fill(.quaternary)
    RoundedRectangle(cornerRadius: 16)
        .fill(.quinary)     // iOS 16.0 이상에서 사용할 수 있다.
}
.foregroundStyle(.blue)
.padding()
```

![다섯 단계의 색상 계층 구조](/assets/img/2025/05/05/image1.png)

`HierarchicalShapeStyle`은 다음과 같은 다섯 단계의 계층 구조를 제공한다:

1. `.primary` - 가장 강한 강도 (기본값)
2. `.secondary` - 두 번째 강도
3. `.tertiary` - 세 번째 강도
4. `.quaternary` - 네 번째 강도
5. `.quinary` - 가장 약한 강도

이 속성들은 자동으로 색상의 다양한 강도를 생성하여 디자인에 깊이를 더해준다. 특히:

- 다크 모드와 테마 변경에 자동으로 적응
- 커스텀 색상에도 동일하게 적용 가능
- 일관된 디자인 시스템 구축 가능

<br>
**참고**
<br>
[HierarchicalShapeStyle | Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/hierarchicalshapestyle){:target="_blank"}