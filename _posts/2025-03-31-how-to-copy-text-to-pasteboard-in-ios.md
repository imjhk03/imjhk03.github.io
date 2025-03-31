---
title: iOS에서 클립보드로 텍스트 복사하는 방법
description: UIPasteboard를 사용하여 텍스트와 다양한 데이터 복사하기
layout: post
categories: [Tips]
---

은행 계좌번호를 복사할 때나 아니면 메시지의 일부 텍스트를 복사하고 싶을 때, 아니면 복사한 문구를 노트에 붙이고 싶을 때 iOS에서 제공하는 `UIPasteboard` 클래스를 통해서 할 수 있다.

## UIPasteboard란?
`UIPasteboard`는 iOS에서 클립보드 콘텐츠를 관리하기 위한 Apple의 시스템이다. 기기의 여러 앱 간에 복사 및 붙여넣기가 작동하도록 담당한다.

가장 일반적인 클립보드 또는 페이스트보드(pasteboard)는 모든 앱이 접근할 수 있는 일반 시스템 페이스트보드이다. 사용자가 한 앱에서 텍스트를 복사하여 다른 앱에 붙여넣을 때 상호 작용하는 곳이다.

## 텍스트 복사하기
아래와 같이 간단하게 텍스트 복사하기 기능을 구현할 수 있다.

```swift
// 기본 사용법
UIPasteboard.general.string = text

// 함수로 만들어 사용하기
func copyToClipboard(_ text: String) {
    UIPasteboard.general.string = text
}
```

## 복사한 텍스트 가져오기
복사한 텍스트를 읽고 싶다면 아래와 같이 옵셔널 언래핑해 가져와서 사용한다.

```swift
if let string = UIPasteboard.general.string {
    // 복사한 텍스트가 있다면 string 상수에 할당이 된다
}
```

## 활용 팁

### 사용자 피드백 추가하기

간단한 햅틱을 추가해서 텍스트가 복사되었는지 사용자에게 인식하게 할 수 있다:

```swift
func copyWithFeedback(_ text: String) {
    // 텍스트 복사하기
    UIPasteboard.general.string = text
    
    // 햅틱 피드백
    let generator = UINotificationFeedbackGenerator()
    generator.notificationOccurred(.success)
}
```

### 여러 항목 복사하기

여러 문구들을 복사하고 싶을 때:

```swift
UIPasteboard.general.strings = ["첫 번째 항목", "두 번째 항목"]
```

### 만료 시간 설정하기

민감한 정보를 다루고 있다면 만료 시간을 설정할 수 있다:

```swift
UIPasteboard.general.setItems([["Sensitive Data": "민감한 데이터"]], 
                             options: [.expirationDate: Date().addingTimeInterval(60)])
```

이 예제는 복사한 콘텐츠가 60초 후에 만료된다.

>민감한 데이터를 복사할 때는 만료 시간을 설정하는 것이 좋다.
{: .prompt-warning}

### 다양한 데이터 타입 복사하기

문자열 외에도 이미지나 URL 주소도 복사할 수 있다:

```swift
// 이미지 복사
UIPasteboard.general.image = UIImage(named: "스크린샷")

// URL 복사
UIPasteboard.general.url = URL(string: "https://apple.com")

// 여러 타입의 데이터 복사
let item: [String: Any] = [
    UIPasteboard.typeListString[0]: "텍스트",
    UIPasteboard.typeListURL[0]: URL(string: "https://apple.com")!
]
UIPasteboard.general.items = [item]
```

<br>
**참고**
<br>
[UIPasteboard | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uipasteboard){:target="_blank"}
