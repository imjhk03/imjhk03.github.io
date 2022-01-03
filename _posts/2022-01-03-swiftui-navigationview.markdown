---
title: SwiftUI에서 NavigationView 사용하기
layout: post
tags: swiftui
---

## NavigationView

SwiftUI에서 [NavigationView](https://developer.apple.com/documentation/swiftui/navigationview)를 이용해서 유저가 navigate 하면서 여러 화면들을 이동할 수 있게 할 수 있습니다. 아래 예시 코드에서 `Text`를 `NavigationView`로 감쌌는데, 감싼 후에 `Text`가 아래로 이동한 것을 볼 수 있습니다.

```swift
struct ContentView: View {
    var body: some View {
        NavigationView {
            Text("Hello, SwiftUI")
        }
    }
}
```

<table>
  <tr>
    <td> <img src="/assets/img/2022/01/03/image1.png" alt="Plain text saying Hello SwiftUI" width="400"> </td>

    <td> <img src="/assets/img/2022/01/03/image2.png" alt="Navigation View is added so the text move below" width="400"> </td>
   </tr> 
   <tr>
      <td>NavigationView가 없는 Text</td>

      <td>NavigationView가 있는 Text </td>
  </tr>
</table>

<!-- <img src="/assets/img/2022/01/03/image1.png" alt="Plain text saying Hello SwiftUI" width="200"/>

<img src="/assets/img/2022/01/03/image1.png" alt="Navigation View is added so the text move below" width="400"/> -->

하지만 네비게이션의 타이틀 영역에 아무것도 안 나타나고 있습니다. 네비게이션의 타이틀을 보여주고 싶다면 `navigationTitle(_:)` modifier를 사용해야 합니다.

## NavigationTitle

네비게이션의 타이틀은 [navigationTitle](https://developer.apple.com/documentation/swiftui/view/navigationtitle(_:)-avgj) modifier를 사용하는데, `NavigationView`의 괄호 바깥에서 지정하는 게 아닌, 안쪽에서 지정해야 합니다.

```swift
// Right Usage
NavigationView {
    Text("Hello, SwiftUI")
        .navigationTitle("SwiftUI")
}

// This does nothing
NavigationView {
    Text("Hello, SwiftUI")
}
.navigationTitle("SwiftUI")
```

<img src="/assets/img/2022/01/03/image3.png" alt="Navigation Title is added, saying SwiftUI" width="400"/>

`NavigationView` 안에 보이고 있는 view의 타이틀을 지정하기 위해서 위에서 Text view에 navigationTitle를 지정합니다. 만약 `NavigationView`에 navigationTitle를 지정하면 고정적인 타이틀을 지정하는 의미로 볼 수 있습니다.

## NavigationLink

네비게이션을 사용하는 이유는 그다음 화면으로 이동하거나 이동한 이후에 전 화면으로 되돌아가거나 등, 유저가 앱을 사용하면서 다양한 화면 이동을 할 수 있도록 하기 위해서입니다. 유저는 SwitUI에서 `NavigationView`에서 다음 이동할 화면을 가기 위해서는 `NavigationLink`를 선택하여 이동합니다.

아래는 다음 화면을 보여주기 위해 텍스트와 색상을 받아서 화면을 그리는 SecondView 입니다. 여기서 navigationTitle를 이용해서 네비게이션의 타이틀을 보여주고, `ZStack`을 이용해서 배경색을 지정하고 그 위에 텍스트를 배치했습니다.

```swift
struct SecondView: View {
    var text: String
    var color: Color

    var body: some View {
        ZStack {
            color
            
            Text(text)
                .font(.body)
        }
        .navigationTitle(text)
    }
}
```

`List` 안에 [`NavigationLink`](https://developer.apple.com/documentation/swiftui/navigationlink)를 이용해서 3개의 항목을 만들고, 각 destination이 SecondView로 이동하도록 합니다. `NavigationLink`는 `Label`과 `Destination`이 필요한데, destination은 이동할 view를 뜻하고 label은 destination으로 이동할 레이블을 뜻합니다.

```swift
struct NavigationLink<Label, Destination> where Label : View, Destination : View
```

```swift
NavigationView {
    List {
        NavigationLink("First", destination: SecondView(text: "First", color: .red))
        NavigationLink("Second", destination: SecondView(text: "Second", color: .green))
        NavigationLink("Third", destination: SecondView(text: "Third", color: .blue))
    }
    .navigationTitle("SwiftUI")
}
```

![A gif showing a three column, and when each selected a second view shows](/assets/img/2022/01/03/image4.gif)

## 마무리

SwiftUI에서는 간단하게 `NavigationView`를 이용해서 유저가 앱을 navigate하면서 화면 이동을 할 수 있게 해줍니다. `NavigationLink`를 이용해서 다음 화면(destination)을 지정하고, `navigationTitle(_:)` modifier를 이용해서 타이틀을 지정할 수 있습니다.

SwiftUI 관련 글을 더 보고 싶다면 [스위프트UI 카테고리 페이지](https://imjhk03.github.io/tags/swiftui/)를 확인해 볼 수 있으며, 질문이나 피드백이 있다면 [트위터](https://twitter.com/_jooheekim_) 혹은 [이메일](mailto:imjhk03@gmail.com)로 연락 주세요.

글 읽어주셔서 감사합니다.