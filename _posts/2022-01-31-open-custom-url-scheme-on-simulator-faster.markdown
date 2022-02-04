---
title: Custom URL Scheme를 시뮬레이터에서 빠르게 여는 방법
layout: post
categories: [Xcode]
tags: [xcode]
---

> 해당 글은 **[Quick way to open a Custom URL Scheme in iOS Simulator](https://sarunw.com/posts/quick-way-to-open-custom-url-scheme-in-ios-simulator/)** 참고하여 작성한 글입니다.

애플에서 커스텀 URL 스키마(이하 custom URL scheme)를 이용하여 앱의 특정 페이지를 여는 방법을 제공합니다. 특정 custom URL scheme을 탭 하면 앱이 열리게 되는데, 테스트하는 방법은 주로 메모 앱에 custom URL scheme 주소들을 저장해서 누르는 방식으로 테스트했습니다. 혹은 사파리에서 직접 주소를 입력하여 앱을 실행했습니다.

<table>
  <tr>
    <td> <img src="/assets/img/2022/01/31/image1.PNG" alt="메모 앱에 4개의 custom URL scheme이 있다" width="400"> </td>

    <td> <img src="/assets/img/2022/01/31/image2.PNG" alt="앱의 일부 화면이 나타나 있는 화면. Custom URL scheme을 지정한 화면 중 하나." width="400"> </td>
   </tr>
</table>

사파리에서 직접 입력하거나 메모 앱에서 하나씩 눌러서 테스트해도 괜찮으나, custom URL scheme이 너무 많을 경우 하나씩 확인하는 작업이 귀찮거나 느릴 수 있습니다.

## 터미널 명령어를 이용해서 시뮬레이터에서 열기

Xcode 안에 있는 도구를 사용할 수 있는 command line인 **`xcrun`**을 사용하면, 시뮬레이터에서 빠르게 custom URL scheme을 열 수 있습니다.

```zsh
xcrun simctl openurl booted {custom URL Scheme}
```

![터미널에 명령어를 입력하여 시뮬레이터에서 앱 열고 있다](/assets/img/2022/01/31/image3.gif)

## 마무리

시뮬레이터에서 빠르게 명령어를 이용해서 custom URL scheme를 여는 방법을 알아보았습니다. 저는 개인적으로 명령어를 사용하니깐 더 빠르게 테스트해서 좋았습니다. 관련하여 애플에서 작성한 **[Defining a Custom URL Scheme for Your App](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app)** 글도 참고하면 좋을 것 같습니다.

Xcode 관련 글을 더 보고 싶다면 [Xcode 카테고리 페이지](https://imjhk03.github.io/tags/xcode/)를 확인해 볼 수 있으며, 질문이나 피드백이 있다면 [트위터](https://twitter.com/_jooheekim_) 혹은 [이메일](mailto:imjhk03@gmail.com)로 연락 주세요.

글 읽어주셔서 감사합니다.