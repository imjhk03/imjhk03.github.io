---
title: SwiftUI에서 View에 Background 추가하기
layout: post
tags: swiftui
---

## View에 Background 더하기

스위프트UI에서 `background(_:alignment:)` view modifier를 이용해서 view에 background를 추가할 수 있습니다. 백그라운드는 추가하는 뷰의 크기만큼 만들어집니다.

```swift
struct ContentView: View {
    let background = Color.blue

    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(Color.green)
            Text("Hello, world!")
                .foregroundColor(Color.white)
                .background(background)
        }
    }

}
```

<img src="/assets/img/2021/12/27/image1.png" alt="Text Hello, world has a blue background" width="400"/>

백그라운드를 추가할 때, 어떤 뷰에 `background(_:alignment:)` modifier를 추가하냐에 따라 그려지는 모습이 달라집니다. 아래는 `VStack`에 백그라운드를 추가한 모습입니다.

```swift
var body: some View {
    VStack {
        Image(systemName: "globe")
            .imageScale(.large)
            .foregroundColor(Color.green)
        Text("Hello, world!")
            .foregroundColor(Color.white)
    }
    .background(background)
}
```

<img src="/assets/img/2021/12/27/image2.png" alt="Image globe and Text Hello, world, wrapped in VStack has a blue background" width="400"/>

## 더 넓은 백그라운드 만들기

만약 더 넓게 백그라운드를 더하고 싶으면, `ZStack` 안에 View를 넣고 백그라운드 뷰를 넣으면 됩니다. 그러면 그 백그라운드는 그리는 뷰 넘어서 크게 그려집니다. 하지만, 기본적으로 safe area 영역을 침범하지 않을 정도로 채웁니다.

```swift
var body: some View {
    ZStack {
        background
        
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(Color.green)
            Text("Hello, world!")
                .foregroundColor(Color.white)
        }
    }
}
```

<img src="/assets/img/2021/12/27/image3.png" alt="A blue background is added to the screen, but the safe area inset is not colored" width="400"/>

## Safe Area까지 백그라운드 넓히기

기본적으로 SwiftUI는 view의 사이즈와 위치가 safe area를 넘지 않도록 조절합니다. 만약 safe area까지 백그라운드를 넓게 그려야 한다면, `ignoresSafeArea(_:edges:)` modifier를 이용하면 됩니다.

```swift
var body: some View {
    ZStack {
        background
        
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(Color.green)
            Text("Hello, world!")
                .foregroundColor(Color.white)
        }
    }
    .ignoresSafeArea()
}
```

<img src="/assets/img/2021/12/27/image4.png" alt="The blue background is extended to the safe area" width="400"/>

## 마무리

SwiftUI에서 아주 간단하게 `background(:alignment:)` modifier를 이용해서 view에 background를 추가할 수 있습니다. `ZStack`을 이용해서 화면 전체에 백그라운드를 추가할 수 있고, safe area까지 넓혀야 한다면 `ignoresSafeArea(_:edges:)` modifier를 이용하여 그릴 수 있습니다.

SwiftUI 관련 글을 더 보고 싶다면 [스위프트UI 카테고리 페이지](https://imjhk03.github.io/tags/swiftui/)를 확인해 볼 수 있으며, 질문이나 피드백이 있다면 [트위터](https://twitter.com/_jooheekim_) 혹은 [이메일](mailto:imjhk03@gmail.com)로 연락 주세요.

글 읽어주셔서 감사합니다.

<br>

Resource

**[Adding a Background to Your View](https://developer.apple.com/documentation/swiftui/adding-a-background-to-your-view)**
